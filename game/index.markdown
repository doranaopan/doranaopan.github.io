---
layout: page
title: ゲーム
permalink: /game/
---

まぁテトリスでもやっていきなよ。

上下左右キーでプレイできるよ。

<!-- テトリスをプレイするボタン -->
<button onclick="openTetris()">テトリスをプレイする</button>

<script>
  function openTetris() {
    window.open('/game/tetris', 'tetrisWindow', 'width=800,height=1600,scrollbars=no,toolbar=no,location=no,status=no');
  }
</script>

<!-- テトリスをプレイするボタン
<a href="/game/tetris" target="_blank">
  <button>テトリスをプレイする</button>
</a> -->
