---
name: moveapps-router
description: "Entry point for MoveApps App development. Use this skill FIRST whenever the user mentions MoveApps or a MoveApp, pastes or attaches R, R Shiny or Python code and wants it turned into a MoveApps App, or describes in plain text an App they want built for MoveApps. It inspects the request, picks exactly one specialised MoveApps skill, and hands over to it with a short brief so the user is not asked twice."
version: 1.1.1
---

# MoveApps router

This skill does not build or convert anything itself. Its only job is to
decide which specialised MoveApps skill in this plugin fits the user's
request, then load that skill and follow it. Load **only** the chosen
skill, never several.
