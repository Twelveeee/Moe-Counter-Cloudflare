# Moe Counter Cloudflare

Language: English | [中文](./README_CN.md)

A cute, multi-theme hit counter deployed with serverless functions.

<p align="center">
  <a href="https://moe-counter-cf.12dev.us/" target="_blank">
    <img alt="Moe Counter Cloudflare!" src="https://moe-counter-cf.12dev.us/@Moe-Counter-Cloudflare.github?name=Moe-Counter-Cloudflare.github&theme=booru-lewd&padding=7&offset=0&align=top&scale=1&pixelated=1&darkmode=auto">
  </a>
</p>

Supports the following 3 deployment modes:

1. Cloudflare Worker + D1
2. Cloudflare Worker + Upstash (REST)
3. Vercel (Node Serverless) + Upstash (REST)

> It is recommended to stay within free-tier limits.
> Use Upstash for sites with monthly PV below 500,000.
> Use D1 for sites with daily PV below 50,000.
> If traffic is higher, consider self-hosting the Docker version of [Moe-Counter](https://github.com/journey-ad/Moe-Counter).

## Demo

https://moe-counter-cf.12dev.us/

For demo only. This public instance may clear data occasionally, so self-hosting is recommended.

## Environment Variables

See `.env.example`.

Common:

- `DB_DRIVER=d1|upstash` (default: `d1`)
- `APP_SITE` (optional)
- `GA_ID` (optional)
- `LOG_LEVEL` (optional)

Required in Upstash mode:

- `UPSTASH_REDIS_REST_URL`
- `UPSTASH_REDIS_REST_TOKEN`
- `COUNTER_PREFIX` (optional, default `moe_count_`)

For Cloudflare D1, this repo declares a D1 binding by `database_name` (without committing `database_id`), so Deploy Button / modern Wrangler can auto-provision and bind `DB`.
If auto-provisioning is unavailable in your flow, manually bind `DB` to your D1 database in the dashboard.

## Deploy 1: Vercel + Upstash
[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2FTwelveeee%2FMoe-Counter-Cloudflare)

1. Create a Redis database in Upstash, then get the REST URL and TOKEN.
2. In your Vercel project, set these environment variables:
   - `DB_DRIVER=upstash`
   - `UPSTASH_REDIS_REST_URL`
   - `UPSTASH_REDIS_REST_TOKEN`
   - `COUNTER_PREFIX` (optional)
   - `APP_SITE` / `GA_ID` / `LOG_LEVEL` (optional)

## Deploy 2: Cloudflare Worker + Upstash
[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https%3A%2F%2Fgithub.com%2FTwelveeee%2FMoe-Counter-Cloudflare)

1. Create a Redis database in Upstash, then get the REST URL and TOKEN.
2. In the Cloudflare dashboard, set:
   - `DB_DRIVER=upstash`
   - `UPSTASH_REDIS_REST_URL`
   - `UPSTASH_REDIS_REST_TOKEN`
   - `COUNTER_PREFIX` (optional)

## Deploy 3: Cloudflare Worker + D1
[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https%3A%2F%2Fgithub.com%2FTwelveeee%2FMoe-Counter-Cloudflare)

1. Click Deploy to Cloudflare and create the Worker.
2. Set `DB_DRIVER=d1` in variables.
3. Confirm a D1 binding named `DB` is present (auto-created in modern flows).
4. Create the table.
![alt text](./docs/image/01.png)

Run SQL in terminal:

[0001_create_tb_count.sql](./migrations/0001_create_tb_count.sql)
```sql
CREATE TABLE IF NOT EXISTS tb_count (
  name TEXT PRIMARY KEY,
  num INTEGER NOT NULL DEFAULT 0 CHECK (num >= 0)
);
```

Click [![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https%3A%2F%2Fgithub.com%2FTwelveeee%2FMoe-Counter-Cloudflare)

![alt text](./docs/image/02.png)
Fill in environment variables, then click create.

If `DB` is not auto-bound in your environment, bind it manually:
1. Add binding:
![alt text](./docs/image/03.png)
2. Select D1 database:
![alt text](./docs/image/04.png)
3. Set variable name to `DB`, then choose your D1 database:
![alt text](./docs/image/05.png)

## Usage Quotas

As of February 18, 2026.

### Vercel free tier

https://vercel.com/docs/limits

1,000,000 requests / month

### Cloudflare Worker free tier

https://developers.cloudflare.com/workers/platform/pricing/

100,000 requests / day

### Upstash free tier

https://upstash.com/docs/redis/overall/pricing

256 MB storage  
500,000 requests / month

### Cloudflare D1 free tier

https://developers.cloudflare.com/d1/platform/pricing/

Reads: 5 million / day  
Writes: 100,000 / day  
Storage: 5 GB

### Quota usage per 100 requests

1. Vercel
![alt text](./docs/image/07.png)

2. Cloudflare Worker
![alt text](./docs/image/08.png)

3. Upstash
![alt text](./docs/image/06.png)

4. Cloudflare D1
![alt text](./docs/image/09.png)

## License

Original repository: [Moe-Counter](https://github.com/journey-ad/Moe-Counter)

[MIT](./LICENSE), excluding theme images.
