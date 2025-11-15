---
title: Copilot Studio Agent Flow Setup Guide
---


# Copilot Studio Agent Flow Setup Guide

## Setup Steps (Tabular Format)

| Step | Description | Screenshot |
|------|-------------|------------|
| 1 | Click on the **Flows** tab (left sidebar). | ![Step 1](/screenshots/step-1.png) |
| 2 | Click on **New agent flow**. | ![Step 2](/screenshots/step-2.png) |
| 3 | Add the **When an agent calls the flow** trigger. | ![Step 3](/screenshots/step-3.png) |
| 4 | Add **Initialize Variable** actions for:<br>- **Space-ID** (Space ID of the Databricks Genie)<br>- **Token** (PAT Token of Databricks)<br>- **Text Output** (stores text output from Databricks Genie)<br>- **Inline Data** (stores query results for visualization) | ![Step 4](/screenshots/step-4.png) |
| 5 | Add an action to send an HTTP **POST** request to the Databricks Genie API to start the conversation:<br>- **URL:** `<Your Databricks endpoint>/api/2.0/genie/spaces/<Space-ID>/start-conversation`<br>- **Header:** `Authorization: Bearer <Token variable>`<br>- **Body:** JSON with `content: User Query` | ![Step 5](/screenshots/step-5.png) |
| 6 | Add an action to send an HTTP **GET** request to Databricks Genie API to get the status of the conversation message (use `conversation_id` and `message_id` from previous step):<br>- **URL:** `<Your Databricks endpoint>/api/2.0/genie/spaces/<Space-ID>/conversations/<conversation_id>/messages/<message_id>`<br>- **Header:** `Authorization: Bearer <Token variable>` | ![Step 6](/screenshots/step-6.png) |
| 7 | Add **Initialize Variable** action to store the status of the conversation message. | ![Step 7](/screenshots/step-7.png) |
| 8 | Add an action to send an HTTP **GET** request to Databricks Genie API to get the status of the conversation message every 5 seconds until the status is **SUCCEEDED**:<br>- **URL:** `<Your Databricks endpoint>/api/2.0/genie/spaces/<Space-ID>/conversations/<conversation_id>/messages/<message_id>`<br>- **Header:** `Authorization: Bearer <Token variable>` | ![Step 8.1](/screenshots/step-8.1.png) <br> ![Step 8.2](/screenshots/step-8.2.png) |
| 9 | Add an action to send an HTTP **GET** request to Databricks Genie API to get the message content:<br>- **URL:** `<Your Databricks endpoint>/api/2.0/genie/spaces/<Space-ID>/conversations/<conversation_id>/messages/<message_id>`<br>- **Header:** `Authorization: Bearer <Token variable>` | ![Step 9](/screenshots/step-9.png) |
| 10 | Iterate over the message contents fetched from the Databricks Genie API:<br>- If the message content type is **text**, store it in the **Text Output** variable. | ![Step 10.1](/screenshots/step-10.1.png) <br> ![Step 10.2](/screenshots/step-10.2.png) |
| 11 | Respond with a structured JSON output to the Agent containing **User Query**, **Text Output**, and **Inline Data**. | ![Step 11](/screenshots/step-11.png) |
| 12 | Click on **Save draft** and then **Publish**. | ![Step 12](/screenshots/step-12.png) |
