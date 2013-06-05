title: SQLServer使用表值参数，高性能批量插入数据
date: 2013-05-30 17:30:11
categories: 学习
tags: SQLServer
---

记得前段时间帮同事写了个解析账号并入库的小工具，来批量导入账号信息，账号量相当大，程序每读取一条记录便执行一次insert来插入数据，整整跑了一下午才把账号全部入库。

<!--more-->

今天又接到同事类似的需求，不过这次的账号量更大，考虑到上次遇到的问题，这次打算采用某种方案来提高插入数据的性能。

了解了下SQLServer批量插入数据的技术，主要有两种：**Bulk**和表值参数(SQLServer 2008的特性)，这两种方式相比循环使用insert插入数据，效率和性能明显上升。使用表值参数带来的提升更为显著。

## 使用表值参数插入数据的一个例子

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using System.IO;
using System.Collections;
using NPOI.HSSF.UserModel;
using System.Data.OleDb;
using System.Data.SqlClient;
using System.Configuration;
using System.Data;

namespace Account2DB
{
    class Program
    {
        public static void TableValuedToDB(DataTable dt)
        {
            SqlConnection sqlConn = new SqlConnection(ConfigurationManager.ConnectionStrings["DefaultConnection"].ConnectionString);
            const string TSqlStatement =
             "insert into NewAccount (Account,Ting,Jie)" +
             " SELECT nc.Account, nc.Ting,nc.Jie" +
             " FROM @BulkTvp AS nc";

            SqlCommand cmd = new SqlCommand(TSqlStatement, sqlConn);
            SqlParameter catParam = cmd.Parameters.AddWithValue("@BulkTvp", dt);
            catParam.SqlDbType = SqlDbType.Structured;
            //表值参数的名字叫BulkUdt，在上面的建立测试环境的SQL中有。  
            catParam.TypeName = "dbo.BulkAccount";
            try
            {
                sqlConn.Open();
                if (dt != null && dt.Rows.Count != 0)
                {
                    cmd.ExecuteNonQuery();
                }
            }
            catch (Exception ex)
            {
                throw ex;
            }
            finally
            {
                sqlConn.Close();
            }
        }

        public static DataTable GetTableSchema()
        {
            DataTable dt = new DataTable();
            dt.Columns.AddRange(new DataColumn[]{  
              new DataColumn("Account",typeof(string)),  
              new DataColumn("Ting",typeof(DateTime)),  
              new DataColumn("Jie",typeof(DateTime))});
            return dt;
        }  

        static void Main(string[] args)
        {
            Logger.Info("正在插入数据，请稍后...");
            try
            {
                StreamReader sr = new StreamReader(args[0], Encoding.Default);
                StringBuilder error = new StringBuilder();
                DataTable dt = GetTableSchema();
                int count = 0;
                while (!sr.EndOfStream)
                {
                    string acc = sr.ReadLine();
                    if (acc.StartsWith("账号") || acc.StartsWith("---") || String.IsNullOrEmpty(acc))
                    {
                        continue;
                    }
                    var data = acc.Split(',');
                    if (data.Count() != 3)
                    {
                        Logger.Error("错误数据：" + acc);
                        error.AppendLine(acc);
                    }
                    DataRow r = dt.NewRow();
                    r[0] = data[0].Trim();
                    r[1] = Convert.ToDateTime(data[1]);
                    r[2] = Convert.ToDateTime(data[2]);
                    dt.Rows.Add(r);
                    count++;
                    // 每一万个账号作为一批账号插入
                    if (count % 10000 == 0)
                    {
                        TableValuedToDB(dt);
                        Logger.Info("已插入：" + count);
                        dt.Clear();
                    }
                }
                // 插入剩余账号
                TableValuedToDB(dt);
                Logger.Info("已插入：" + count);

                sr.Close();
                File.AppendAllText(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "error.txt"), error.ToString(), Encoding.Default);
            }
            catch (Exception ex)
            {
                Logger.Error(ex.ToString());
            }
            Logger.Info("执行结束，按任意键关闭...");
            Console.ReadLine(); 
        }
    }
}

```
