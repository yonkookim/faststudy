# Practice 05. 정렬과 필터 적용하기

<a href="https://fastcampus-all.netlify.com/2020-spring/04-javascript/final-practice" target="_blank">최종 완성본 보기</a>
<br>
<a href="https://gitlab.com/junseol86/fastcampus-lecture-codes/-/raw/master/2020-spring/04-javascript/practice-05/practice-05.zip" target="_blank">
  [실습파일 다운로드 받기]
</a>
<br>
<br>

## 정렬 적용해보기
> 최신순으로 정렬
```javascript
photos = photos.sort(function (a, b) {
  return (a.idx > b.idx) ? -1 : 1 }
})
```
> 좋아요 많은 순으로 정렬
```javascript
photos = photos.sort(function (a, b) {
  return (a.likes > b.likes) ? -1 : 1 }
})
```

<br>

## 필터 적용해보기
> 필터링된 리스트 따로 두기
```javascript
var filtered;
```
> 모든 사진
```javascript
var filtered = photos.filter(function (it) {
  return true;
});
```
> 내 사진
```javascript
var filtered = photos.filter(function (it) {
  return it.user_id === my_info.id;
});
```
> 내가 좋아요 한 사진
```javascript
var filtered = photos.filter(function (it) {
  return my_info.like.indexOf(it.idx) > -1;
});
```
> 내가 팔로우하는 사용자의 사진
```javascript
var filtered = photos.filter(function (it) {
  return my_info.follow.indexOf(it.user_id) > -1;
});
```

<br>

## 정렬과 필터 방식 변수로 지정
```javascript
var sort = function (a, b) { return (a.idx > b.idx) ? -1 : 1 };
var filter = function (it) { return true; };
```
```javascript
function showPhotos () {
  // ...
  var filtered = photos.filter(filter);
  filtered.sort(sort);
  // ...
}
```

<br>

***

<br>

## 정렬과 필터를 버튼에 적용하려면?
* 원하는 정렬이나 필터를 선택할 시 리스트가 새로 나타나야 한다.  
  * 해당 정렬이나 필터가 적용된 채 `showPhotos()` 가 실행되어야 함  

<br>

* 정렬을 바꾸더라도 필터가, 필터를 바꾸더라도 정렬이 **유지**되어야 한다.  
  * 선택된 정렬과 필터가 변수값으로 따로 저장되어 있어야 함.  

### 알고리즘
```
왼쪽 탭의 버튼으로 정렬이나 필터를 선택하면  
외부 변수로 해당 방식들이 지정된 뒤  
이들에 따라 정렬, 필터링을 하여 리스트를 보여준다.
```

<br>

***

<br>


## 정렬과 필터 방식 외부 변수로 두기
> 정렬 방식을 객체로 지정
```javascript
var sorts = {
  recent: function (a, b) { return (a.idx > b.idx) ? -1 : 1 },
  like: function (a, b) { return (a.likes > b.likes) ? -1 : 1 }
}
```
> 현재 선택된 정렬
```javascript
var sort = sorts.recent;
```

<br>

> 필터 방식을 객체로 지정
```javascript
var filters = {
  all: function (it) { return true; },
  mine: function (it) { return it.user_id === my_info.id; },
  like: function (it) { return my_info.like.indexOf(it.idx) > -1; },
  follow: function (it) { return my_info.follow.indexOf(it.user_id) > -1; }
}
```
> 현재 선택된 필터
```javascript
var filter = filters.all;
```

<br>

## `setSort` 함수
> 함수 지정
```javascript
function setSort(_sort) {
  // ...
}
```
> 버튼에 이벤트 지정
```html
  <ul id="sorts" class="dep _gallery">
    <li class="recent on" onclick="setSort('recent')">최신순 보기</li>
    <li class="like" onclick="setSort('like')">좋아요 순 보기</li>
  </ul>
```
> 버튼 상태 변화
```javascript
function setSort(_sort) {
  var sortButtons = document.querySelectorAll("#sorts li");
  sortButtons.forEach(function (sortButton) {
    sortButton.classList.remove('on');
  })
  document.querySelector("#sorts ." + _sort).classList.add("on");
  // ...
}
```
> 선택된 정렬 방식 적용
```javascript
function setSort(_sort) {
  // ...
  sort = sorts[_sort];
  showPhotos();
}
```

<br>

## `setFilter` 함수
> 함수
```javascript
function setFilter(_filter) {
  var filterButtons = document.querySelectorAll("#filters li");
  filterButtons.forEach(function (filterButton) {
    filterButton.classList.remove('on');
  });
  document.querySelector("#filters ." + _filter).classList.add("on");
  filter = filters[_filter];
  showPhotos();
}
```
> 버튼 이벤트 지정
```html
  <ul id="filters" class="dep _gallery">
    <li class="all on" onclick="setFilter('all')">전체 사진</li>
    <li class="mine" onclick="setFilter('mine')">내 사진 사진</li>
    <li class="like" onclick="setFilter('like')">좋아요 한 사진</li>
    <li class="follow" onclick="setFilter('follow')">팔로우 회원 사진</li>
  </ul>
```