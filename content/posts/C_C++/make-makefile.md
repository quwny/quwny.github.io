---
title: "Make Makefile"
date: 2022-09-24T20:39:54+08:00
lastmod: 2022-09-24T20:39:54+08:00
author: ["Quwny"]
tags: 
- make
- makefile
description: ""
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
slug: ""
comments: true
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true # 顶部显示当前路径
draft: false # 草稿
---

## Make

什么是 Make

## Makefile

什么是 Makefile

有什么用？

## 通用 Makefile 文件

```makefile
############### Makefile Common ###############
# C or C++
EXT = c
# gcc or g++
CC = gcc
# includes directory
INCDIR = includes
# sources directory
SRCDIR = sources
# target directory
TARDIR = target
# .o file directroy
OBJDIR = $(TARDIR)/obj
# .d file directory
DEPDIR = $(TARDIR)/dep
# exec filename
EXEC = $(TARDIR)/main.exe

# 预处理选项
CPPFLAGS += -I$(INCDIR)

# link
LDFLAGS +=

#  编译器选项
CFLAGS += -O -std=c11 -g -Wall

SRCS = $(foreach dir, $(SRCDIR), $(wildcard $(dir)/*.$(EXT)))
OBJS = $(patsubst %.$(EXT), $(OBJDIR)/%.o, $(notdir $(SRCS)))
DEPS = $(patsubst %.$(EXT), $(DEPDIR)/%.d, $(notdir $(SRCS)))

all: $(EXEC)
	@echo Successful

$(EXEC): $(OBJS)
	@$(CC) $^ -o $@

ifneq "$(MAKECMDGOALS)" "clean"
sinclude $(DEPS)
endif

$(OBJDIR)/%.o: $(SRCDIR)/%.$(EXT)
	@$(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o $@

$(DEPDIR)/%.d: $(SRCDIR)/%.$(EXT)
	@$(CC) $(CFLAGS) $(CPPFLAGS) -MM -MF $@ $< -MT $(DEPDIR)/$*.d -MT $(OBJDIR)/$*.o

.PHONY: all clean

clean:
	@del /Q /S $(TARDIR)
```
