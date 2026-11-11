# 更新记录

## 1.2.2（2026-11-11，已发布）

已发布到 [npm 官方仓库](https://www.npmjs.com/package/@gdpisen/idock-cli/v/1.2.2)，`latest` 指向 `1.2.2`。

- `search` 新增 `--time today/7d/last-week` 和带时区偏移的 `--from/--to`，透传用户时区；按录制时间过滤，区间含起点、不含终点。
- JSON 保留来源 `recording_id/title/recorded_at`，录音编号按字符串继续用于读取、下载和导出；text 显示来源标题、录制时间和片段正文。
- 无匹配或低相关性时 text 提示“未找到相关内容。”，不引用候选正文，不把检索分数当作答案正确率。
- 将服务端同类参数错误统一映射为 `INVALID_ARGUMENT`，退出码 2。
- 同步命令帮助、schema、交互帮助和 Skill 工作流；升级 CLI 后需更新已安装 Skill。
- 全量 139 项测试通过，并在隔离目录安装最终 tarball，验证版本、帮助、schema 和三个 Skill 文件；官方 registry 的 integrity 与本地发布包一致。
- 需要配套新版后端；本次发布不代表线上业务服务已经部署，也不宣称真实检索准确率已达到特定比例。

## 1.2.1（2026-11-06，已发布）

已发布到 [npm 官方仓库](https://www.npmjs.com/package/@gdpisen/idock-cli/v/1.2.1)，发布时 `latest` 指向 `1.2.1`。

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
