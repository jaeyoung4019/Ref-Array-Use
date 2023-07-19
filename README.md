# Ref-Array-Use

퍼블리싱 변경을 하기 위해서 ref 를 주로 사용해야 하는 경우가 있습니다. ex : 스타일 컴포넌트를 사용하지 않고 퍼블리싱 받은 클래스들을 사용할 때 리액트용으로 변경 할 때,

이 때에 문제 되는 것이 리스트를 API 요청으로 받아와서 가변적으로 사용해야할 때 Ref 를 잡아야할 때가 있습니다.

예를 들면

```ts
<li class="card_item type_pf">
    <div class="item_hd">
        <div class="item_chk">
            <div class="cus_inp_box chk_inp">
                <label for="list_item_chk2" class="label_box"><span class="sound_only">item_check</span>
                    <input type="checkbox" id="list_item_chk2" name="list_item_chking">
                    <span class="inp_custom"></span>
                </label>
            </div>
        </div>
        <div class="flex_wrap">
            <div class="pf_img_box"><img src="../../resources/img/icon/user_thumb.png" alt=""></div>
            <div class="info_subject">
                <div class="subject_line"><span class="tit user_name">test_dev</span></div>
                <div class="info_type">Guest</div>
            </div>
        </div>
    </div>
    <div class="item_bd">
        <div class="drop_box">
            <div class="drop_btn"><span class="btn_txt">Collaborator</span><i class="icon arrow arrow_d"></i></div>
            <div class="drop_menu">
                <ul class="drop_list">
                    <li class="drop_item"><a href="#none">Maintainer</a></li>
                    <li class="drop_item"><a href="#none">Collaborator</a></li>
                    <li class="drop_item"><a href="#none">Guest</a></li>
                    <li class="drop_item warning"><a href="#none">Drop member</a></li>
                </ul>
            </div>
        </div>
    </div>
</li>
<li class="card_item type_pf">
    <div class="item_hd">
        <div class="item_chk">
            <div class="cus_inp_box chk_inp">
                <label for="list_item_chk3" class="label_box"><span class="sound_only">item_check</span>
                    <input type="checkbox" id="list_item_chk3" name="list_item_chking">
                    <span class="inp_custom"></span>
                </label>
            </div>
        </div>
        <div class="flex_wrap">
            <div class="pf_img_box"><img src="../../resources/img/icon/user_thumb.png" alt=""></div>
            <div class="info_subject">
                <div class="subject_line"><span class="tit user_name">test_dev</span></div>
                <div class="info_type">Guest</div>
            </div>
        </div>
    </div>
    <div class="item_bd">
        <div class="drop_box">
            <div class="drop_btn"><span class="btn_txt">Collaborator</span><i class="icon arrow arrow_d"></i></div>
            <div class="drop_menu">
                <ul class="drop_list">
                    <li class="drop_item"><a href="#none">Maintainer</a></li>
                    <li class="drop_item"><a href="#none">Collaborator</a></li>
                    <li class="drop_item"><a href="#none">Guest</a></li>
                    <li class="drop_item warning"><a href="#none">Drop member</a></li>
                </ul>
            </div>
        </div>
    </div>
</li>

```

이렇게 li 태그가 반복되는 부분에

```ts
   <div class="drop_box">
      <div class="drop_btn"><span class="btn_txt">Collaborator</span><i class="icon arrow arrow_d"></i></div>
          <div class="drop_menu">
              <ul class="drop_list">
                  <li class="drop_item"><a href="#none">Maintainer</a></li>
                  <li class="drop_item"><a href="#none">Collaborator</a></li>
                  <li class="drop_item"><a href="#none">Guest</a></li>
                  <li class="drop_item warning"><a href="#none">Drop member</a></li>
              </ul>
          </div>
      </div>
  </div>
```

이런 식으로 펑션이 들어가야 하는 경우

한 개일 경우에는 ref 를 지정하여 처리하면 되지만 리스트라면 ref 또한 list 로 아래처럼 생성해서 사용해야합니다. 

```ts

 const dropBoxRef = useRef<any>([]);

    const dropBtnOnClick = (e: any , index: any) => {
        const dropMenu = dropMenuRef.current[index];
        const dropBox = dropBoxRef.current[index];
        if (dropMenu != null && dropBox != null){
            const dropMenuClassList = dropMenu.className.split(" ");
            const dropMenuClose = "drop_menu";
            const dropMenuOpen = "drop_menu open";
            const dropBoxClose = "drop_box";
            const dropBoxOpen = "drop_box open";
            if (dropMenuClassList.includes("open")) {
                dropMenu.className = dropMenuClose
                dropBox.className = dropBoxClose
            } else {
                dropMenu.className = dropMenuOpen
                dropBox.className = dropBoxOpen
            }
        }
        e.stopPropagation();
    }

    const hideMenu = (index: any) => {
        const dropMenu = dropMenuRef.current[index];
        const dropBox = dropBoxRef.current[index];
        if (dropMenu != null && dropBox != null) {
            const dropMenuClose = "drop_menu";
            const dropMenuOpen = "drop_menu open";
            const dropBoxClose = "drop_box";
            const dropBoxOpen = "drop_box open";
            dropMenu.className = dropMenuClose
            dropBox.className = dropBoxClose
        }
    }
```

리스트로 온다면 ref 를 계속 선언해서 사용해야할 것입니다. 그래서 배열에 담아놓고 사용하는 방법을 사용해야 하는데, 이런 식으로 el 을 넣어줌으로 써 해결할 수 있습니다.

```ts
               <div className="drop_box" ref={(el) => dropBoxRef.current[index] = el}>
                                        <div className="drop_btn" onClick={ (e) => dropBtnOnClick( e , index)}>
                                            <span className="btn_txt">{value?.role_nm}</span>
                                            <i className="icon arrow arrow_d"></i>
                                        </div>
                                        <div className="drop_menu" ref={(el) => dropMenuRef.current[index] = el}>
```
