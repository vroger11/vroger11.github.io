---
comments: true
title:  "My participation to Hackaviz"
date:   2025-04-07
tags:
    - Development
    - Data Vizualization
    - Python
hide:
    - navigation
excerpt: My participation to the 2025 hackaviz competition.
---

Hackaviz is an annual open data visualization challenge hosted by Toulouse DataViz.
I participated to this 2025 edition.
It is organized by the awesome folks at **Toulouse DataViz**.
Given the data from the competition, I created an interactive tool to explore water levels and rainfall in the Toulouse region.
Here’s a breakdown of the steps I took to create the visualization.

## 🎯 My goals

Before starting I wanted to define my goals which were:

- create interaction between at least two graphs (I wanted to learn that skill using Plotly and Streamlit and experimenting).
- having fun doing it.
- Submit something that looked finished and thought through.

## 🔍 The Exploration Phase

Like most data science projects, it all started with some basic yet important steps to better understand the data:

- Running `df.describe()` across variables helped flag odd min/max values and high variances.
- Simple line plots by date exposed trends, gaps, and a few strange outliers.
- I used these first steps to **detect missing data**, normalize station values, and prepare for a more guided experience.

## 🚀 The Final App

My app is named Toulouse Water level & Rainfall Explorer. The goal to help users understand how water levels and rainfall interact in Toulouse using real open data. Here are a few links to my creation:

- [Live app](https://vroger11-hackaviz-2025.streamlit.app/)
- [Source code](https://github.com/vroger11/hackaviz-2025)
- [Demo video](https://www.youtube.com/watch?v=2wekZIu_DFI)

The visualization is composed of two plots, a water level linechart and the associated rainfall map.
With the first graph, you can select a range of points to filter the dates used for the rainfall map.
Also, you can use filter from the sidebar to select a date range for the water level observations in Toulouse and adjust the top N rainfall stations to display on the map.

## 🧠 Lessons & Learnings

Things I am happy with:

- avoided classic errors with outliers of the data.
- delivered a working and interactive app.
- my solution can handle many point (nearly 150 years of data can be shown on my first plot).
- done and learned how to do interaction between two Plotly plots in a Streamlit app (completed my personal goal).

Things I can improve:

- my capacity to create more appealing visualizations (using better visual elements and improve the design).
- as many data dates were missing for the rainfall, I could color the first graph points with blue for rainfall data available and stay in grey otherwise (instead of acceleration).
- create a multi-langue version.

A big thanks to the **Toulouse DataViz** association for organizing Hackaviz 2025 and providing such rich datasets.
Hackaviz was a great opportunity to engage with real-world data and explore new visualization ideas.

---

*Let me know what you think, I hope it was useful for you.*
See you later,
Vincent
