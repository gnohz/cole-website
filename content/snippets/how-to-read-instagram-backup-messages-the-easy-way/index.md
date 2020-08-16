---
title: "How to Read Instagram Backup Messages the Easy Way - jq"
date: 2020-08-16T15:43:09-04:00
draft: false
tags: ["jq"]
---

A little bit ago I deleted my instagram account (not the app, my actual account). Before doing so I made sure to backup my data thinking that I might want to look back on it in the future. I was right. Recently I wanted to look back through some messages, but it was not easy to read; the json format that Instagram provided on a single line, and the file was too big for vim to handle. Here is a snippet of what it would look like:

```
[{"participants": ["person1", "person2", "person3"], "conversation": [{"sender": "person3", "created_at": "2020-07-12T14:30:19.783799+00:00", "text": "Our very own group chat!"}, {"sender": "person2", "created_at": "2020-07-09T12:05:49.033851+00:00", "text": "Wow this is exciting"}, {"sender": "person1", "created_at": "2020-07-08T06:51:20.897859+00:00", "likes": [{"username": "person2", "date": "2020-07-08T16:38:07.925968+00:00"}], "text": "Hey everyone welcome to the groupchat!"}]}]
```

This is where `jq` came in handy.

{{< highlight bash >}}
cat messages.json | jq --color-output '.[] 
    | (.conversation  
        | . as $orig 
        | length as $len 
        | [keys[] 
        | $orig[$len - 1 - .]]) as $new 
    | .conversation |= $new' | less
{{</ highlight >}}

While not much, we're already getting somewhere. This formats, colorcodes, and sortes the conversations chronologically.

```
{
  "participants": [
    "person1",
    "person2",
    "person3"
  ],
  "conversation": [
    {
      "sender": "person1",
      "created_at": "2020-07-08T06:51:20.897859+00:00",
      "likes": [
        {
          "username": "person2",
          "date": "2020-07-08T16:38:07.925968+00:00"
        }
      ],
      "text": "Hey everyone welcome to the groupchat!"
    },
    {
      "sender": "person2",
      "created_at": "2020-07-09T12:05:49.033851+00:00",
      "text": "Wow this is exciting"
    },
    {
      "sender": "person3",
      "created_at": "2020-07-12T14:30:19.783799+00:00",
      "text": "Our very own group chat!"
    }
  ]
}
```

Next, if you want to focus in on a single converation you had with a person you can run the following:

{{< highlight bash >}}
cat messages.json | jq --color-output '.[] 
    | if .participants == ["person1", "person2", "person3"]
    then (.conversation  
        | . as $orig 
        | length as $len 
        | [keys[] 
        | $orig[$len - 1 - .]]) as $new 
    | $new | .[] | [.created_at, .sender, .text] | @csv
    else empty
    end' | less

{{</ highlight >}}


This outputs the conversation as a csv with columns for timestamp, user, and text. You can open the output csv in your favorite csv viewer.

```
"\"2020-07-08T06:51:20.897859+00:00\",\"person1\",\"Hey everyone welcome to the groupchat!\""
"\"2020-07-09T12:05:49.033851+00:00\",\"person2\",\"Wow this is exciting\""
"\"2020-07-12T14:30:19.783799+00:00\",\"person3\",\"Our very own group chat!\""
```

Hope this helps you to rediscover your Instagram dms!
