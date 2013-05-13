title: 使用C语言解析文本文件
date: 2013-03-31
categories: 学习
tags: [C语言]
---

## 起因

又是周末了，事情多啊，我的一名研究生同学，最近在整理论文资料，让我帮忙将一个格式混乱的Excel文件，写个程序调整成按列存储的格式。

刚刚把Linux弄好，还没来得及装Windows，平时最擅长的语言是C#，可是同学比较急，无奈，就在Linux写个C程序来解析吧。

<!-- more -->
## 原始格式(2条示例信息)

	1,,,,,,
	,,,,,,
	Workflow process modelling and resource allocation based on polychromatic sets theory,,,,,,   SCI
	Gao, Xinqin (School of Mechanical and Precision Instrument Engineering, Xi'an University of Technology, Xi'an 710048, China); Xu, Lida; Wang, Xueping; Li, Yan; Yang, Mingshun; Liu, Yong  Source: Enterprise Information Systems, v 7, n 2, p 198-226, May 2013,,,,,,  
	Database: Compendex,,,,,,
	Abstract | Detailed | |,,,,,,
	,,,,,,
	2,,,,,,
	,,,,,,
	A new cooperative spectrum sensing scheme for cognitive Ad-Hoc networks,,,,,,   SCI
	Du, Yang (School of Automation Science and Electrical Engineering, Beihang University, Beijing, China); Li, Hongxiang; Lin, Weiyao; Liu, Lingjia; Wang, Xudong; Khan, Samee U.; Wu, Sentang  Source: Mobile Networks and Applications, v 17, n 6, p 746-757, December 2012, Mobility in Support of Social Communication,,,,,,
	Database: Compendex,,,,,,
	Abstract | Detailed | |,,,,,,
	,,,,,,

## 目标格式(2条示例信息)

	1,Workflow process modelling and resource allocation based on polychromatic sets theory,SCI,Gao_ Xinqin ; Xu_ Lida; Wang_ Xueping; Li_ Yan; Yang_ Mingshun; Liu_ Yong, Enterprise Information Systems,2013
	2,A new cooperative spectrum sensing scheme for cognitive Ad-Hoc networks,SCI,Du_ Yang ; Li_ Hongxiang; Lin_ Weiyao; Liu_ Lingjia; Wang_ Xudong; Khan_ Samee U.; Wu_ Sentang, Mobile Networks and Applications,2012

## 解析代码

	/*
	 * 翟士丹，2013/3/31,论文信息格式化处理工具
	 */
	#include <stdio.h>
	#include <sys/types.h>
	#include <string.h>
	#include <regex.h>

	#define MAX_LENGTH 1024

	int main(int argc, char *argv[])
	{
		if (argc != 3) {
			fprintf(stderr, "usage: format srcfile desfile\n");
			return 0;
		}

		FILE* src;
		FILE* des;
		
		src = fopen(argv[1], "r");
		des = fopen(argv[2], "w");

		char buf[MAX_LENGTH] = {0};		// 原始记录缓冲区
		char result[MAX_LENGTH] = {0}; 	// 记录处理结果
		char tempbuf[MAX_LENGTH] = {0};	// 临时处理缓冲区
		int num = 0;			// 记录行号

		// 正则表达式相关
		regex_t reg;
		char* regexp = "[0-9]{4}"; 	// 奇怪,\\d{4}怎么就不行呢
		int err, status;
		char errbuf[MAX_LENGTH];
		int nm = 1;
		regmatch_t pmatch[nm];

		// 编译正则表达式
		if (regcomp(&reg, regexp, REG_EXTENDED) < 0) {
			regerror(err, &reg, errbuf, sizeof(errbuf));
			fprintf(stdout, "err:%s\n", errbuf);
		}

		// 记录查找的字符位置
		char* ptr;
		char* temp_ptr;

		while (fgets(buf, MAX_LENGTH, src) != NULL) {
			fprintf(stdout, "%d:%s", num + 1, buf);
			if (strncmp(buf, ",,,,,,", 6) == 0 ||
			    strncmp(buf, "Abstract", 8) == 0 ||
			    strncmp(buf, "Database", 8) == 0) {
				continue;
			}

			num++;
			// 解析处理
			if (num == 1 || num == 2) {
				ptr = strchr(buf, ',');
				strncat(result, buf, ptr-buf+1);
			}
			if (num == 2) {
				ptr = strstr(buf, ", ");
				if (ptr != NULL) {
					ptr++;
					while(*ptr == ' ') {
						ptr++;
					}
					strncat(result, ptr, strlen(ptr)-1);
				}
				strcat(result, ",");
			} 
			if (num == 3) {
				ptr = strchr(buf, '(');
				temp_ptr = buf;
				if (ptr != NULL) {
					strncat(tempbuf, buf, ptr-buf);
					temp_ptr = strchr(ptr, ')')+1;
				}
				ptr = strstr(temp_ptr, "  Source: ");
				strncat(tempbuf, temp_ptr, ptr-temp_ptr);
				// 将人名中的逗号替换
				int i = 0;
				for (i; tempbuf[i]; i++) {
					if (tempbuf[i] == ',') {
						tempbuf[i] = '_';
					}
				}
				// 拼接论文来源
				strcat(tempbuf, ",");
				temp_ptr = ptr+10;
				ptr = strchr(temp_ptr, ',');
				strncat(tempbuf, temp_ptr, ptr-temp_ptr+1);
				// 拼接发布时间
				temp_ptr = ptr+1;
				for (i=0; i<3; i++) {
					temp_ptr = strchr(temp_ptr, ',') + 1;
				}
				// 通过正则表达式提取时间
				status = regexec(&reg, temp_ptr, nm, pmatch, REG_NOTBOL);
				if (status == REG_NOMATCH)
					printf("no match\n");
				if (status == 0) {
					strncat(tempbuf, temp_ptr + pmatch[0].rm_so, pmatch[0].rm_eo - pmatch[0].rm_so);
				}
				strcat(result, tempbuf);
			}
			if (num == 3) {
				fprintf(des, "%s\n", result);
				fprintf(stdout, "%s\n", result);
				memset(result, 0, sizeof(result));
				memset(tempbuf, 0, sizeof(tempbuf));
				num = 0;
			}
			memset(buf, 0, sizeof(buf));
		}
		regfree(&reg);
		fclose(src);
		fclose(des);
		return 0;
	}
