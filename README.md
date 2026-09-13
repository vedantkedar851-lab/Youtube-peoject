# Youtube-project
![Youtube project](https://github.com/vedantkedar851-lab/Youtube-peoject/blob/main/youtube-logo-featured.webp)

# YouTube Trending Video & Engagement Analysis

##  Project Overview

Analysis of YouTube trending videos using PostgreSQL to identify
high-performing videos, channels, audience engagement patterns,
and important content performance metrics.

##  Business Objectives

- Identify top-performing videos
- Analyze channel performance
- Measure audience engagement
- Calculate engagement and like-to-view rates
- Identify high-performing content
## Dataset 
[youtube Dataset](https://www.kaggle.com/datasets/elijahconnectng/youtube-trending-video?utm_source=chatgpt.com)

##  Business Problems
## 1 — Which videos received the highest number of views?
```SQOSELECT 
    title,
    channel_title,
    views
FROM youtube_trending
ORDER BY views DESC
LIMIT 10;
```
Helps identify the type of content and channels capable of generating high audience reach.

## 2 — Which YouTube channels have the most trending videos?
```SELECT 
    channel_title,
    COUNT(*) AS trending_videos
FROM youtube_trending
GROUP BY channel_title
ORDER BY trending_videos DESC
LIMIT 10;
```
Identifies channels that consistently produce content capable of reaching the trending list.

## 3 — Which channels generate the highest average views?
```
SELECT 
    channel_title,
    ROUND(AVG(views), 0) AS avg_views
FROM youtube_trending
GROUP BY channel_title
HAVING COUNT(*) >= 5
ORDER BY avg_views DESC
LIMIT 10;
```
Shows channels with consistently strong audience reach.

## 4 — Which videos receive the most likes?
```
SELECT
    title,
    channel_title,
    likes
FROM youtube_trending
ORDER BY likes DESC
LIMIT 10;
```
Helps identify content generating strong positive audience interaction.

## 5 — What is the average engagement by channel?
```
SELECT
    channel_title,
    ROUND(AVG(
        likes + dislikes + comment_count
    ), 0) AS avg_engagement
FROM youtube_trending
GROUP BY channel_title
HAVING COUNT(*) >= 5
ORDER BY avg_engagement DESC
LIMIT 10;
```
Identifies channels whose audiences actively interact with their content.

## 6 — Which videos have the highest engagement rate?
```
SELECT
    title,
    channel_title,
    views,
    ROUND(
        ((likes + dislikes + comment_count)::NUMERIC 
        / NULLIF(views, 0)) * 100,
        2
    ) AS engagement_rate
FROM youtube_trending
WHERE views > 10000
ORDER BY engagement_rate DESC
LIMIT 10;
```
A video with fewer views can still have a highly engaged audience.

## 7 — Which videos generate the most comments?
```
SELECT
    title,
    channel_title,
    comment_count
FROM youtube_trending
WHERE comments_disabled = FALSE
ORDER BY comment_count DESC
LIMIT 10;
```
Identifies content that encourages discussion and audience participation.

## 8 — What percentage of videos have comments disabled?
```
SELECT
    ROUND(
        100.0 * SUM(
            CASE 
                WHEN comments_disabled = TRUE THEN 1
                ELSE 0
            END
        ) / COUNT(*),
        2
    ) AS comments_disabled_percentage
FROM youtube_trending;
```
Helps understand how frequently creators restrict audience interaction.

## 9 — Which channels have the best like-to-view ratio?
```
SELECT
    channel_title,
    ROUND(
        100.0 * SUM(likes) / NULLIF(SUM(views), 0),
        2
    ) AS like_rate
FROM youtube_trending
GROUP BY channel_title
HAVING SUM(views) > 100000
ORDER BY like_rate DESC
LIMIT 10;
```
Measures how effectively channels convert viewers into positive interactions.

## 10 — Rank videos by views within each channel
```
SELECT
    channel_title,
    title,
    views,
    RANK() OVER (
        PARTITION BY channel_title
        ORDER BY views DESC
    ) AS video_rank
FROM youtube_trending;
```
## Tools used

- PostgreSQL
- SQL
- Kaggle Dataset
- GitHub



...
