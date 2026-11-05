# 更新记录

## 1.2.1（2026-11-05，待发布）

- 录音读取、下载和导出明确使用录音列表返回的会议主键 `id`；不能使用听悟 `taskId`、导入 `job_id` 或纪要 `recordId`。
- 主键按十进制字符串传递，校验后端 Long 范围，拒绝已丢失精度的数字输入。大整数主键在详情、两种音频下载及各类文档导出中保持精确。
- `--help` 和 `schema` 补充主键来源与参数说明。
- CLI 版本统一读取安装包清单，保证命令、响应和 HTTP 请求头版本一致。
- 完整转写 TXT/Markdown 沿用临时授权下载和本地转换；CLI 继续使用专用接口及个人 AccessKey。
- 本地全量验证 137 项通过；覆盖 HTTP/OSS 替身及独立 CLI 进程，本次未进行线上业务联调。

升级命令：

```sh
npm install -g @gdpisen/idock-cli@latest --registry=https://registry.npmjs.org/
```

升级后按 [安装指南](README.md) 重新运行当前 Agent 的 Skill 安装器；已有有效登录可以继续使用。
