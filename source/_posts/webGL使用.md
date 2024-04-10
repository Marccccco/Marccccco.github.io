---
title: webGL使用
---

### [webGL 使用](https://docs.unity.cn/cn/2020.3/Manual/webgl-interactingwithbrowserscripting.html)

#### js 跟 unity 通讯

1.打包完成后在生成的.html 文件里这样写(unity2020.1 版本之后)

```html
<script>
	var loaderUrl = "Build/webgl.data";
	var myGameInstance = null;
	var canvas = document.querySelector("#unity-canvas");
	var script = document.createElement("script");
	script.src = loaderUrl;
	var config = {
		dataUrl: "Build/webgl.data",
		frameworkUrl: "Build/webgl.framework.js",
		codeUrl: "Build/webgl.wasm",
		streamingAssetsUrl: "StreamingAssets",
		companyName: "DefaultCompany",
		productName: "NanDingGDS",
		productVersion: "0.1.0",
	};
	script.onload = () => {
		createUnityInstance(canvas, config, (progress) => {}).then(
			(unityInstance) => {
				myGameInstance = unityInstance;
			}
		);
	};
	document.body.appendChild(script);

    function js2Unity() {
        // 第一个参数是unity中物体的名称,第二是要调用的方法名称,第三个参数是unity中接收到的参数
        myGameInstance.SendMessage("Main Camera", "TestRotation", '');
    }
</script>
```
#### untiy 调用 js
1.请使用 .jslib 扩展名将包含 JavaScript 代码的文件放置在 Assets 文件夹中的“Plugins”子文件夹下
```js
mergeInto(LibraryManager.library, {

  Hello: function () {
    window.alert("Hello, world!");
  },

  HelloString: function (str) {
    window.alert(UTF8ToString(str));
  },

  PrintFloatArray: function (array, size) {
    for(var i = 0; i < size; i++)
    console.log(HEAPF32[(array >> 2) + i]);
  },

  AddNumbers: function (x, y) {
    return x + y;
  },

  StringReturnValueFunction: function () {
    var returnStr = "bla";
    var bufferSize = lengthBytesUTF8(returnStr) + 1;
    var buffer = _malloc(bufferSize);
    stringToUTF8(returnStr, buffer, bufferSize);
    return buffer;
  },

  BindWebGLTexture: function (texture) {
    GLctx.bindTexture(GLctx.TEXTURE_2D, GL.textures[texture]);
  },

});
```
C#代码
```c#
using UnityEngine;
using System.Runtime.InteropServices;

public class NewBehaviourScript : MonoBehaviour {

    [DllImport("__Internal")]
    private static extern void Hello();

    [DllImport("__Internal")]
    private static extern void HelloString(string str);

    [DllImport("__Internal")]
    private static extern void PrintFloatArray(float[] array, int size);

    [DllImport("__Internal")]
    private static extern int AddNumbers(int x, int y);

    [DllImport("__Internal")]
    private static extern string StringReturnValueFunction();

    [DllImport("__Internal")]
    private static extern void BindWebGLTexture(int texture);

    void Start() {
        Hello();
        
        HelloString("This is a string.");
        
        float[] myArray = new float[10];
        PrintFloatArray(myArray, myArray.Length);
        
        int result = AddNumbers(5, 7);
        Debug.Log(result);
        
        Debug.Log(StringReturnValueFunction());
        
        var texture = new Texture2D(0, 0, TextureFormat.ARGB32, false);
        BindWebGLTexture(texture.GetNativeTexturePtr());
    }
}
```

