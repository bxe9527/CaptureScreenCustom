# CaptureScreenCustom
自定义区域屏幕截图

void Start()
    {

        Vector3[] img4 = new Vector3[4];
        image.transform.GetComponent<RectTransform>().GetWorldCorners(img4);
        for (int i = 0; i < img4.Length; i++)
        {
            Debug.Log(img4[i]);
        }
        //这边是选择了一个图片内区域作为截图内容
        StartCoroutine(CutSpriteFromScreen(img4[0],img4[2], (i) => { image.sprite = i; }));
    }

    //选择一块区域截图并生成一个sprite。
    public  IEnumerator CutSpriteFromScreen( Vector2 min,Vector2 max, UnityAction<Sprite> complete)
      {
        Vector2 sp = min;

        Vector2 temp = max;
          Vector2Int size = new Vector2Int((int)(temp.x - sp.x), (int)(temp.y - sp.y));
  
          //判断截图框是否超出屏幕边界
          if (sp.x< 0 || sp.y< 0 || sp.x + size.x> Screen.width || sp.y + size.y> Screen.height)
              yield break;
 
         //等待当前帧渲染结束，此为必须项
         yield return new WaitForEndOfFrame();
 
         Texture2D texture = new Texture2D(size.x, size.y, TextureFormat.RGB24, false);
 
         texture.ReadPixels(new Rect(sp.x, sp.y, size.x, size.y), 0, 0, false);
         texture.Apply();
 
         complete(Sprite.Create(texture, new Rect(Vector2Int.zero, size), Vector2.one* .5f));
     }
