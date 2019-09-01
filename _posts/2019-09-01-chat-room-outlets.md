---
title: "'NSUnknownKeyException' 오류"
elements:
  - content
  - css
  - formatting
  - html
  - markup
categories:
  - Swift
tags:
  - Swift
  - Outlets
---


채팅 앱 개발을 하던 도중 PerformSegue(withIdentifier:,sender:) 를 이용하여 뷰를 이동하던중 'NSUnknownKeyException' 오류가 발생했다.

처음에는 무슨 오류인지 몰라서 'this class is not key value coding-compliant for the key item.' 라는 키워드로 검색을 하니

스토리보드에서 참조하고 있던 오브젝트가 사라져서 생기는 오류라고 해서
곰곰히 생각해보니 ChatRoomViewController.swift 에 여러 아웃렛 변수를 연결 해 두었는데 생각없이 삭제를해서 생긴 오류였다....

![info-plist](/images/chatRoomOutlets.png)

위 와 같이 아웃렛을 깔끔히 비운뒤 실행하니 제대로 실행이 됐다.
다음부턴 이런 삽질 안하도록 해야겠다.
