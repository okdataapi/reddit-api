# Reddit 数据接口（Reddit API）：帖子搜索、评论、社区、用户、热门

**Reddit 数据 API · Reddit 帖子搜索 · Reddit 评论采集 · Reddit 社区热帖 · Reddit 用户 · Reddit 视频字幕。**

这个仓库是 [okdata](https://okdata.online/zh/?src=gh-reddit-api) 的Reddit接口示例：11 个接口在线可用，一个 API Key 调用，按次计费（$0.002 起），失败不扣费。不用登录平台账号，不用 Cookie，不开浏览器，所以也不存在账号被风控的问题。

[免费注册拿 Key（送 $0.1 试用额度）](https://okdata.online/zh/signup/?src=gh-reddit-api) · [全部接口文档](https://okdata.online/zh/apis/?src=gh-reddit-api) · [接入 AI（MCP）](https://okdata.online/zh/mcp/?src=gh-reddit-api)

## 接口清单

| 接口 | 说明 | 价格 | 路径 |
|---|---|---|---|
| [Reddit 帖子搜索](https://okdata.online/zh/apis/reddit/posts/search/) | 在全站按关键词搜帖子，可按相关、最新或最高分排，可限定发布时间：标题、正文、作者、社区、得分和评论数。 | $0.004 / 次 | `GET /v1/reddit/posts/search` |
| [Reddit 帖子详情](https://okdata.online/zh/apis/reddit/posts/get/) | 按 ID 或链接取一条帖子：标题、正文、作者、社区、得分、赞同比例和评论数。 | $0.002 / 次 | `GET /v1/reddit/posts/get` |
| [Reddit 帖子评论](https://okdata.online/zh/apis/reddit/posts/comments/list/) | 帖子下的评论，连同前几层回复（parent_id 表示回复的是哪条）：内容、评论者、得分和回复数。按 Reddit 默认顺序、最新或最高分排。 | $0.002 / 次 | `GET /v1/reddit/posts/comments/list` |
| [Reddit 热门帖子](https://okdata.online/zh/apis/reddit/popular/posts/list/) | Reddit「Popular」热门流里正在热的帖子，从最热开始：标题、社区、得分和评论数。 | $0.002 / 次 | `GET /v1/reddit/popular/posts/list` |
| [Reddit 社区帖子](https://okdata.online/zh/apis/reddit/subreddits/posts/list/) | 某个社区的帖子，按热门、最新或最高分排。最高分默认只排最近一天发的帖子，可用 published_within 改成一周、一个月、一年或全部。 | $0.004 / 次 | `GET /v1/reddit/subreddits/posts/list` |
| [Reddit 社区资料](https://okdata.online/zh/apis/reddit/subreddits/get/) | 把社区（subreddit）当作账号返回：标题、简介、成员数（在 followers 里）、创建时间，以及每周访客和发帖数。 | $0.002 / 次 | `GET /v1/reddit/subreddits/get` |
| [Reddit 趋势搜索](https://okdata.online/zh/apis/reddit/trends/list/) | Reddit 今天的热门搜索词，按 App 里的顺序。 | $0.002 / 次 | `GET /v1/reddit/trends/list` |
| [Reddit 用户资料](https://okdata.online/zh/apis/reddit/users/get/) | 用户的公开资料：显示名、简介、认证、karma、发帖数和评论数，以及注册时间。 | $0.002 / 次 | `GET /v1/reddit/users/get` |
| [Reddit 用户帖子](https://okdata.online/zh/apis/reddit/users/posts/list/) | 用户发过的帖子，从新到旧：标题、正文、社区、得分和评论数。 | $0.002 / 次 | `GET /v1/reddit/users/posts/list` |
| [Reddit 用户活跃社区](https://okdata.online/zh/apis/reddit/users/subreddits/list/) | 用户活跃的社区，每个当作账号返回：社区名、简介和成员数（在 followers 里）。一次返回全部。 | $0.002 / 次 | `GET /v1/reddit/users/subreddits/list` |
| [Reddit 视频字幕](https://okdata.online/zh/apis/reddit/posts/transcript/get/) | Reddit 自己托管的视频帖子的字幕，带时间戳。没有字幕的帖子返回 not_found，不收费。 | $0.004 / 次 | `GET /v1/reddit/posts/transcript/get` |

## 快速开始

1. 在 [okdata 控制台](https://okdata.online/zh/signup/?src=gh-reddit-api) 用邮箱注册，拿到 `okd_` 开头的 Key。
2. 调一次接口：

```bash
export OKDATA_API_KEY=okd_...
curl -G "https://api.okdata.online/v1/reddit/posts/search" \
  -H "Authorization: Bearer $OKDATA_API_KEY" \
  --data-urlencode 'query=saturn rings'
```

Python 版在 [examples/quickstart.py](examples/quickstart.py)（只用标准库，会顺带演示翻页）：`python3 examples/quickstart.py`。

返回统一的结构：列表在 `data.items`，下一页用 `page.next_cursor` 传回 `cursor`，这次扣了多少钱在 `meta.billing.amount`，平台不提供的字段为 null 并在 `meta.unavailable` 里写明原因。

## 让 AI 直接用（MCP）

Claude Code：

```bash
claude mcp add --transport http --scope user okdata https://api.okdata.online/mcp --header "Authorization: Bearer $OKDATA_API_KEY"
```

Cursor、Cherry Studio、WorkBuddy 等支持远程 MCP 的客户端：

```json
{
  "mcpServers": {
    "okdata": {
      "url": "https://api.okdata.online/mcp",
      "headers": {
        "Authorization": "Bearer okd_..."
      }
    }
  }
}
```

接好以后直接用中文说需求，比如「Reddit 帖子搜索：saturn rings」。AI 会先用免费的 `find`、`describe` 找接口和看价格，再用 `call` 取数据。

## 常见问题

**要登录平台账号吗？** 不用。数据是公开内容，okdata 在服务端获取，你的账号完全不参与。

**怎么收费？** 每个接口单独定价，表里写的就是一次调用的价格；按条计费的接口按实际返回条数算。调用失败（包括超时）不扣费。新账号送 $0.1 试用额度，之后按需充值，没有订阅。价格页：https://okdata.online/zh/pricing/

**和官方开放平台有什么区别？** 官方开放平台只能读授权给你的账号自己的数据，而且要企业资质和审核；这里是平台上任何人都能看到的公开数据，比如搜索结果、别人的笔记或视频、评论、热榜。

**能翻页吗？** 列表接口每页一次调用，把 `page.next_cursor` 原样传回 `cursor` 就是下一页，24 小时内有效。

**出错了怎么办？** 每个错误都有固定的 `code`、中文说明 `message_zh`，以及会不会扣费（`charged`）和要不要重试（`retry`）。全部错误码：https://okdata.online/zh/docs/errors/

## English

Examples for the okdata Reddit API: 11 live endpoints behind one API key, pay per call from $0.002, failed calls free, no platform login or cookies. Sign up at https://okdata.online/signup/?src=gh-reddit-api, docs at https://okdata.online/apis/, MCP at https://okdata.online/mcp/.

## License

MIT. The data comes from the okdata API; its terms apply to API use.
