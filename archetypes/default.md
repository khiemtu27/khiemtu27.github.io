---
title: {{ replace .Name "-" " " | title }}
description:
tags:
    - book
    - literature
    - music
    - tap-am
    - tech
    - thoughts
date: {{ now.Format "2006-01-02" }}
slug: {{ .Name }}
---