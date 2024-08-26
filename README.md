# Hexo 博客项目

欢迎来到我的 Hexo 博客项目！这是一个使用 Hexo 框架构建的静态博客，旨在提供一个高效且易于管理的个人博客解决方案。

## 目录

- [Hexo 博客项目](#hexo-博客项目)
  - [目录](#目录)
  - [介绍](#介绍)
  - [安装](#安装)
  - [配置](#配置)

## 介绍

Hexo 是一个快速、简洁且高效的静态博客框架，支持 Markdown 格式的文章编写和多种主题、插件的扩展。这个项目旨在展示如何使用 Hexo 创建和管理博客。

## 安装

确保你的系统已经安装了 Node.js。如果还没有安装，请访问 [Node.js 官网](https://nodejs.org/) 下载安装。  

1. **安装 Hexo CLI**

   打开终端或命令提示符，运行以下命令安装 Hexo CLI：

   ```bash
   npm install -g hexo-cli
2. **克隆项目**

   将本项目克隆到本地：

   ```bash
   git clone https://github.com/你的用户名/你的项目名.git
   cd 你的项目名
3. **安装依赖**
   在项目目录下运行以下命令安装所需的 npm 包：
   ```bash
   npm install
## 使用
1. **创建新文章**
   使用 Hexo CLI 创建新文章：
   ```bash
   hexo new "文章标题"  
   ```
   新文章将会被创建在 source/_posts/ 目录下。
2. **生成静态文件**
   在项目目录下，运行以下命令生成静态文件：
   ```bash
   hexo generate
   ```
3. **启用本地服务**
   启动本地服务器以预览博客：
   ```bash
   hexo server
   ```
   访问 http://localhost:4000 查看博客。
## 配置
项目的配置文件位于 _config.yml。你可以在这个文件中设置网站的基本信息、主题、插件等配置。