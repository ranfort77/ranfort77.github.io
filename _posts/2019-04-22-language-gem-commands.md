---
title: "ruby gem 관련 명령어"
excerpt: "ruby gem 기억해야 할 명령어를 기록"
categories:
  - language
tags:
  - ruby
  - gem
last_modified_at: 2019-04-22T01:47
---


참고 링크
* https://rubygems.org/
* [gem 관련 명령어](https://cholchori.tistory.com/1337)

## 원격저장소 gem 찾기

원격저장소에 gem이 있는지 이름으로 찾기( [Finding Gems](<https://guides.rubygems.org/rubygems-basics/#finding-gems>))

```bash
$ gem search ^rails
(...생략...)
```

## gem 설치 확인

```bash
$ gem list
(...생략...)

$ gem list -d bundler
bundler (2.0.1)
    Authors: Andr챕 Arko, Samuel Giddins, Colby Swandale, Hiroshi
    Shibata, David Rodr챠guez, Grey Baker, Stephanie Morillo, Chris
    Morris, James Wen, Tim Moore, Andr챕 Medeiros, Jessica Lynn Suttles,
    Terence Lee, Carl Lerche, Yehuda Katz
    Homepage: http://bundler.io
    License: MIT
    Installed at: C:/Ruby25-x64/lib/ruby/gems/2.5.0

    The best way to manage your application's dependencies
```

## gem 삭제

```bash
gem uninstall <gem-name>
```



