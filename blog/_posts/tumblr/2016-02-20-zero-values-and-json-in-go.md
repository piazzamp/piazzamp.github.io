---
layout: post
title: Zero Values and JSON in Go
date: '2016-02-20T19:41:58-06:00'
tags:
- golang
- json
tumblr_url: http://mattpiazza.com/post/139690985270/zero-values-and-json-in-go
---
Go is great, I think that is apparent. If you have talked to me about software development at all in the past 5 months then you have certainly heard me talk about Go and how pleasing I find it to write.

There are great, baked-in features, like documentation, testing, and (obviously) JSON encoding & decoding of native types. It’s very simple to enable - any exported attribute (a field marked public, if you’re java-kin) of a struct can be encoded by the encoding/json package and will give you the obvious result where all struct elements have identical JSON field names. You have the option to override this default behavior and rename the fields or omit them entirely if they’re empty. So if your user has no middleName then the JSON version of that person will just not have a middleName field. Sound strightforward?

How does encoding/json determine which fields to omit? I can’t say that I have read through their implementation, but it is safe to say that the package omits any field that has the zero-value for its type. That is a 0-length string, the int 0, float 0.0, or false. This works sometimes. If you want to pass back a false or a 0, your JSON representation will simply not have the fields you have set to those values.

tl;dr: make sure there is no case where you want to send a zero-value via JSON in a field marked omitempty
