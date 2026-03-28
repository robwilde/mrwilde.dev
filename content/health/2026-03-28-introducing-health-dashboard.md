---
title: "Introducing the Health Dashboard"
date: 2026-03-28
draft: false
layout: "post"
tags: ["health", "automation", "quantified-self"]
authors: ["Robert Wilde"]
description: "Building an automated health tracking and reporting system with AI agents, Exist.io, and Hugo."
---

## Why This Exists

I've been tracking macros and glucose readings for a while now, but it's always been scattered — a photo here, a reading there, data spread across apps with no single picture. So I built something to fix that.

This section of the site is an **automated health dashboard**. Every week, my data gets pulled together, formatted into a report, and published here. No manual effort once it's running. The goal is simple: **public accountability** and a single place to see trends over time.

## The System

The whole thing runs on two self-hosted AI agents built with [OpenClaw](https://github.com/nichochar/openclaw):

**NutriBot** handles nutrition. I send it a photo of my meal via Telegram, it identifies the food, estimates macros (Protein, Carbs, Fat, Calories), and logs everything to Google Calendar. It also takes voice notes and blood glucose readings. It's been running for a couple of weeks now and has become the fastest way I've ever tracked food.

**HealthBot** is the newer agent. It pulls in data from **Exist.io** — which aggregates metrics from my Aurora ring (sleep), Google Fit (activity), RescueTime (productivity), and Android. It also queries NutriBot for nutrition data. Every Sunday, it generates a weekly report, drafts a blog post, sends me a preview via Telegram, and on my approval, publishes it here automatically — commit to GitHub, build with Hugo, deploy via Dokploy. Fully hands-off after I say "publish".

## What's Being Tracked

Here's the full picture of data sources flowing into the dashboard:

- **Sleep** — Aurora ring: duration, deep/light/REM stages, HRV, resting heart rate, bedtime consistency
- **Activity** — Google Fit: steps, active minutes, floors, energy burned
- **Productivity** — RescueTime: productive vs distracting time, screen time breakdown
- **Nutrition** — NutriBot: meal photo analysis with per-meal Protein/Carbs/Fat/Calories
- **Blood Glucose & Ketones** — Manual readings via Telegram (I'm keto-focused, so glucose and GKI matter)
- **Weight** — Morning fasted weigh-ins
- **Custom Tags** — 24 boolean daily trackers in Exist.io: boxing, gym, yoga, code day, eating well, migraine, back pain, and more

## Early Data

The system just came online this week, so data is still filling in. Here's what we have so far:

**Glucose readings** from NutriBot (mid-March baseline):

| Date | Glucose | Context |
|------|---------|---------|
| Mar 17 | 6.0 mmol/L | Evening |
| Mar 18 | 5.4 mmol/L | Post-dinner |
| Mar 19 | 6.1 mmol/L | Final reading |

Sitting in the **5.4–6.1 mmol/L range** — reasonable for evening/post-meal readings on keto. Fasted morning readings are the real metric I'll be tracking week over week.

**Productivity** from RescueTime is flowing: ~2,400 minutes of productive time logged in the first few days. Sleep and activity data from the Aurora ring and Google Fit are still being connected to Exist.io — expect those in next week's report.

And for those curious, the signature **Avocado Coffee** that starts most days: 150g coconut cream, 135g avocado, 68g egg, 250ml water, 25g protein powder. Comes in at **~936 kcal, ~35g protein, ~77g fat, ~21g carbs**. It's a meal, not a beverage.

## What to Expect

Starting next Sunday, automated weekly posts will appear here with:

- **Data tables** for each category (sleep, activity, productivity, body composition) with week-over-week trends
- **Correlation insights** from Exist.io (e.g., "more steps correlates with better sleep")
- **A short reflection** on the week's wins and areas for improvement

The data gets richer every week. Follow along if you're into quantified self, keto, or just want to see what happens when you let AI agents publish your health data.
