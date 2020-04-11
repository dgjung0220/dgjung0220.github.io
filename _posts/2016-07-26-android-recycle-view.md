---
tags: ["android"]
title: "[android] RecyclerView 에서의 setOnItemClickListener (흉내내기)"
date: 2016.07.26
category: 'android'
---

### 개요  

* TOC
{:toc}


RecyclerView는 ListView에서의 비효율적인 리스트 아이템 생성과 같은 점을 극복하고자 좀 더 유연하고 좀 더 효율적으로 사용되기 위해 만들어졌다고 합니다.  
RecyclerView이 만들기 이전에도 ListView에서 ViewHolder개념을 응용? 도입? 해서 효율적으로 사용하였지만 RecyclerView에선 강제적으로 사용하게 되었다고 합니다.  

하지만 Listview에서 setOnItemClickListener 와 같은 기능을 지원하지 않습니다.  addOnItemTouchListener와 같은 기능이 있지만 listview에서와 같이 직접적으로 position에 접근을 할 수 없고 여러 이슈가 있다고 합니다.  

### 흉내내보자  

RecyclerView에서 onBindViewHolder해줄 때에 각각의 아이템에 OnClickListener를 등록해 주는 것으로 접근했습니다.  
코드는 핵심적인 부분만 중략해서 작성했습니다.  

```java
public class RecyclerAdapter extends RecyclerView.Adapter{
    private ArrayList strings;

    //아이템 클릭시 실행 함수
    private ItemClick itemClick;
    public interface ItemClick {
        public void onClick(View view,int position);
    }

    //아이템 클릭시 실행 함수 등록 함수
    public void setItemClick(ItemClick itemClick) {
        this.itemClick = itemClick;
    }

    public static class ViewHolder extends RecyclerView.ViewHolder{
        View view;
        public ViewHolder(View view) {
            super(view);
            this.view = view;
        }
    }

    @Override
    public RecyclerAdapter.ViewHolder onCreateViewHolder(ViewGroup parent,
                                                         int viewType) {
        // create a new view
        View v = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.list_items, null);
        ViewHolder vh = new ViewHolder(v);

        return vh;
    }


    //중략 ...................

    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        final int Position = position;
        //중략 ...................
        holder.view.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if(itemClick != null){
                    itemClick.onClick(v, Position);
                }
            }
        });
    }
    //중략 ...................

}
```

adapter 부분에서 이와 같은 방식으로 작성하였습니다.  
이후에 recycler 호출부분에서 아래와 같이 작성한다면, listview에서 setOnItemClickListener 와같이 작동할 수 있습니다.  
```java
        mAdapter.setItemClick(new RecyclerAdapter.ItemClick() {
            @Override
            public void onClick(View view, int position) {
                //클릭시 실행될 함수 작성
            }
        });
```

코드가 길어져 상당히 중략을 하였습니다.  

제가 작성한 예제 코드는  
안드로이드 스터디 프로젝트 [https://github.com/ignocide/android_study/](git)에 공유하고 있습니다.  
