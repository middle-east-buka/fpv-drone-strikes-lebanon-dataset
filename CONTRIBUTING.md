# Contributing — adding videos to the dataset

This dataset's media lives in a **private** S3 bucket (`fpv-drone-strikes-lebanon-dataset`)
and is served publicly through CloudFront. Public links must use the CloudFront domain
`d2fioemadmrru3.cloudfront.net` — direct S3 URLs are blocked (HTTP 403) by design.

## Naming

Use `YYYY-MM-DD_short_description` (lowercase, underscores). Use the **same base name**
for the video and its thumbnail.

## 1. Upload the files to S3

You need AWS credentials with write access to the bucket (already configured on the
upload machine).

```bash
NAME="2026-06-22_merkava_tank_example"          # date_description
aws s3 cp "$NAME.mp4" "s3://fpv-drone-strikes-lebanon-dataset/videos/$NAME.mp4"
aws s3 cp "$NAME.jpg" "s3://fpv-drone-strikes-lebanon-dataset/thumbnails/$NAME.jpg"
```

Keep the `videos/` and `thumbnails/` prefixes exactly.

## 2. Add a row to `README.md`

Newest entry goes at the **top** of the table. Links go through CloudFront, not S3:

```markdown
| 2026-06-22 | <img src="https://d2fioemadmrru3.cloudfront.net/thumbnails/2026-06-22_merkava_tank_example.jpg" alt="Merkava tank example" width="180"> | Merkava tank example | [Download](https://d2fioemadmrru3.cloudfront.net/videos/2026-06-22_merkava_tank_example.mp4) |
```

## 3. Commit & push

```bash
git add README.md && git commit -m "Add 2026-06-22 merkava tank example" && git push
```

## Important

- Always use the `d2fioemadmrru3.cloudfront.net` domain in links.
- **Do not** make the bucket public or add public-read policies. Abuse protection
  (per-IP rate limiting + a cost alert) depends on the bucket staying private behind
  CloudFront.
