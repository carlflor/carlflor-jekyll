---
layout: post
title: Create a Sprite for UIImage from a Web Request
slug: unity-spi-sprite
category: unity
permalink: notes/:slug
---

Here's a code snippet of how you can load an image from a web request to a sprite,
and assign it to a UIImage afterwards:

``` java

// at the top of the file:
using UnityEngine.Networking;

// inside the class declaration:

public Image uiImage;

void Start() {
  StartCoroutine (GetTexture ());
}

IEnumerator GetTexture() {
  UnityWebRequest www = UnityWebRequest.GetTexture("http://www.topvinylfilms.com/images/products/preview/meme074.jpg");

  yield return www.Send();

  if(www.isError) {
    Debug.Log(www.error);
  }
  else {
    Texture2D webTexture = ((DownloadHandlerTexture)www.downloadHandler).texture as Texture2D;
    Sprite webSprite = SpriteFromTexture2D (webTexture);
    uiImage.sprite = webSprite;


  }
}

Sprite SpriteFromTexture2D(Texture2D texture) {

  return Sprite.Create(texture, new Rect(0.0f, 0.0f, texture.width, texture.height), new Vector2(0.5f, 0.5f), 100.0f);
}

```
When you do call a web request, it is best to handle it through a coroutine
so you can handle concurrency well. In this example, I've used a url of an image hosted online.
The DownloadHandlerTexture is used to get the actual image from the web request that was
previously done.

<br>

When the web request is finished, I create a sprite from the texture downloaded. Sprite.Create
might not be as optimal, but since my app won't be using it too much, I opted to used it.
