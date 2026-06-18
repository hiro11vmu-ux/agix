// LINEユーザーID確認用 Webhook (Vercel Serverless Function)
//
// 仕組み: LINEから届いたメッセージイベントの source.userId を読み取り、
// そのまま「あなたのユーザーIDはこちらです」という返信メッセージとして
// 本人に送り返す（reply API使用）。これによりサーバー側で何も保存せず、
// LINEのトーク画面上で直接ユーザーIDを確認できる。

export default async function handler(req, res) {
  if (req.method !== "POST") {
    res.status(200).send("This endpoint expects LINE webhook POST requests.");
    return;
  }

  const events = req.body?.events || [];
  const channelAccessToken = process.env.LINE_CHANNEL_ACCESS_TOKEN;

  for (const event of events) {
    console.log("LINE event:", JSON.stringify(event));

    if (event.type === "message" && event.replyToken && event.source?.userId) {
      const userId = event.source.userId;
      const replyText =
        "あなたのユーザーIDはこちらです:\n" + userId +
        "\n\nこれをコピーして保存してください。";

      try {
        await fetch("https://api.line.me/v2/bot/message/reply", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            Authorization: "Bearer " + channelAccessToken,
          },
          body: JSON.stringify({
            replyToken: event.replyToken,
            messages: [{ type: "text", text: replyText }],
          }),
        });
      } catch (err) {
        console.error("Reply failed:", err);
      }
    }
  }

  res.status(200).send("OK");
}
