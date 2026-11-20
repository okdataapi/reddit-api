# Reddit API: post search, comments, subreddits, users and trending

**Reddit data API · Reddit post search · Reddit comments and threads · subreddit posts · Reddit user profiles · Reddit trending searches.**

Examples for the [okdata](https://okdata.online/?src=gh-reddit-api) Reddit API: 11 live endpoints behind one API key, pay per call from $0.002. No Reddit login, no cookies and no browser, so there is no account to get blocked.

[Sign up free ($0.10 of trial credit)](https://okdata.online/signup/?src=gh-reddit-api) · [All Reddit endpoints](https://okdata.online/apis/reddit/?src=gh-reddit-api) · [Use it from Claude, Cursor or any MCP client](https://okdata.online/mcp/?src=gh-reddit-api)

## Endpoints

| Endpoint | What it returns | Price | Path |
|---|---|---|---|
| [Reddit post search](https://okdata.online/apis/reddit/posts/search/?src=gh-reddit-api) | Posts matching keywords across Reddit, by relevance, newest or top score, optionally within a period: title, text, author, community, score and comment count. | $0.004 a call | `GET /v1/reddit/posts/search` |
| [Reddit post detail](https://okdata.online/apis/reddit/posts/get/?src=gh-reddit-api) | One post by ID or link: title, full text, author, community, score, upvote ratio and comment count. | $0.002 a call | `GET /v1/reddit/posts/get` |
| [Reddit post comments](https://okdata.online/apis/reddit/posts/comments/list/?src=gh-reddit-api) | Comments on a post with the first levels of replies (parent_id says which comment a reply answers): text, commenter, score and reply count. Reddit's default order, newest or top score first. | $0.002 a call | `GET /v1/reddit/posts/comments/list` |
| [Reddit Popular feed](https://okdata.online/apis/reddit/popular/posts/list/?src=gh-reddit-api) | The posts on Reddit's Popular feed right now, hottest first: title, community, score and comment count. | $0.002 a call | `GET /v1/reddit/popular/posts/list` |
| [Reddit community posts](https://okdata.online/apis/reddit/subreddits/posts/list/?src=gh-reddit-api) | A community's posts, hot, newest or top score first. Top score covers the last day unless published_within asks for a week, month, year or all time. | $0.004 a call | `GET /v1/reddit/subreddits/posts/list` |
| [Reddit community profile](https://okdata.online/apis/reddit/subreddits/get/?src=gh-reddit-api) | A community (subreddit) as an account: title, description, members (in followers), when it was created, and weekly visitors and contributions. | $0.002 a call | `GET /v1/reddit/subreddits/get` |
| [Reddit trending searches](https://okdata.online/apis/reddit/trends/list/?src=gh-reddit-api) | The searches trending on Reddit today, in the order the app lists them. | $0.002 a call | `GET /v1/reddit/trends/list` |
| [Reddit user profile](https://okdata.online/apis/reddit/users/get/?src=gh-reddit-api) | A user's public profile: display name, bio, verification, karma, post and comment counts and when the account was created. | $0.002 a call | `GET /v1/reddit/users/get` |
| [Reddit user posts](https://okdata.online/apis/reddit/users/posts/list/?src=gh-reddit-api) | The posts a user has made, newest first: title, text, community, score and comment count. | $0.002 a call | `GET /v1/reddit/users/posts/list` |
| [Reddit user active communities](https://okdata.online/apis/reddit/users/subreddits/list/?src=gh-reddit-api) | The communities a user is active in, each as an account: name, description and members (in followers). The whole list in one answer. | $0.002 a call | `GET /v1/reddit/users/subreddits/list` |
| [Reddit video transcript](https://okdata.online/apis/reddit/posts/transcript/get/?src=gh-reddit-api) | The captions of a Reddit-hosted video post, with timestamps. A post without captions comes back not_found and is charged like any lookup. | $0.004 a call | `GET /v1/reddit/posts/transcript/get` |

## Quick start

1. Sign up at the [okdata console](https://okdata.online/signup/?src=gh-reddit-api) with your email and copy the key that starts with `okd_`.
2. Make a call:

```bash
export OKDATA_API_KEY=okd_...
curl -G "https://api.okdata.online/v1/reddit/posts/search" \
  -H "Authorization: Bearer $OKDATA_API_KEY" \
  --data-urlencode 'query=saturn rings'
```

The Python version is [examples/quickstart.py](examples/quickstart.py) (standard library only, and it fetches a second page too): `python3 examples/quickstart.py`.

Every endpoint answers in the same shape: the list is in `data.items`, `page.next_cursor` goes back as `cursor` for the next page, what the call cost is in `meta.billing.amount`, and a field the platform does not provide is null with the reason in `meta.unavailable`.

## Use it from AI (MCP)

Claude Code:

```bash
claude mcp add --transport http --scope user okdata https://api.okdata.online/mcp --header "Authorization: Bearer $OKDATA_API_KEY"
```

Cursor, Windsurf, VS Code and other clients with remote MCP:

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

Then ask in plain words, for example "Reddit post search: saturn rings". The agent uses the free `find` and `describe` tools to pick the endpoint and see its price, then `call` to fetch the data.

## FAQ

**Do I need a Reddit account?** No. The data is public content, fetched on the server; your own account is never involved.

**How is it billed?** Each endpoint has its own price, listed above for one call; endpoints billed per item count what comes back. New accounts get $0.10 to try; after that, top-ups start at $10 and never expire, with no subscription. Pricing: https://okdata.online/pricing/?src=gh-reddit-api · Billing details: https://okdata.online/docs/billing/?src=gh-reddit-api

**Can I page through results?** Each page of a list is one call: pass `page.next_cursor` back as `cursor` for the next one. A cursor stays valid for 24 hours.

**What happens when a call fails?** Every error has a fixed `code`, a `message`, and says whether you were charged (`charged`) and whether to retry (`retry`). All error codes: https://okdata.online/docs/errors/?src=gh-reddit-api

## 中文说明

**Reddit 数据 API · Reddit 帖子搜索 · Reddit 评论采集 · Reddit 社区热帖 · Reddit 用户 · Reddit 视频字幕。**

okdata 的Reddit接口示例：11 个接口在线可用，一个 API Key 调用，按次计费（$0.002 起），不用登录平台账号、不用 Cookie。中文文档：https://okdata.online/zh/apis/reddit/?src=gh-reddit-api · 注册：https://okdata.online/zh/signup/?src=gh-reddit-api · 接入 AI（MCP）：https://okdata.online/zh/mcp/?src=gh-reddit-api

## License

MIT. The data comes from the okdata API; its terms apply to API use.
