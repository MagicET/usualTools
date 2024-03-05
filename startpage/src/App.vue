<script setup>
import { ref, computed } from 'vue';
import Bookmark from './Bookmark.vue';
import ReadingList from './ReadingList.vue';

const urlBox = ref("");

const search = computed(() => {
  return "https://www.google.com/search?q=" + urlBox.value + ""
});

const readingLists = ref([
  "https://google.com", "https://google.co.jp"
]);

const bookmarks = ref([
  { name: "Google",text: "https://google.com", icon: "🔍" }, 
  { name: "Bing", text: "https://bing.com", icon: "🌐" }, 
  { name: "Apple", text: "https://apple.com", icon: "🍎" }, 
  { name: "Amazon", text: "https://amazon.co.jp", icon: "🛒" }
]);

function setBookmark() {
  let newBookmark = { name: urlBox.value, text: urlBox.value, icon: "🌐" };
  bookmarks.value.push(newBookmark);
}

function addReadingList() {
  readingLists.value.push(urlBox.value);
}

const isBookmarkUnpushable = computed(() => {
  let result = urlBox.value == ""
  result = result || bookmarks.value.some((bookmark) => bookmark.text == urlBox.value);
  return result;
})

const isReadingListUnpushable = computed(() => {
  let result = urlBox.value == ""
  result = result || readingLists.value.includes(urlBox.value);
  return result;
})

function deleteBookmark(url) {
  const bookmarkEditDialog = document.getElementById("bookmarkEditDialog");
  bookmarks.value = bookmarks.value.filter((bookmark) => bookmark.text != url);
  bookmarkEditDialog.close();
}

function deleteReadingList(url) {
  readingLists.value = readingLists.value.filter((readingList) => readingList != url);
}

const editingBookmark = ref("");
const newUrl = ref("");
const newIcon = ref("");
const newName = ref("")
function openBookmarkDialog(url, icon, name) {
  const bookmarkEditDialog = document.getElementById("bookmarkEditDialog");
  newUrl.value = url;
  newName.value = name;
  newIcon.value = icon;
  editingBookmark.value = url;
  bookmarkEditDialog.showModal()
}
function closeBookmarkDialog() {
  const newBookmark = { name: newName.value, text: newUrl.value, icon: newIcon.value };
  const bookmarkIndex = bookmarks.value.findIndex((bookmark) => bookmark.text == editingBookmark.value);
  bookmarks.value[bookmarkIndex] = newBookmark;
  const bookmarkEditDialog = document.getElementById("bookmarkEditDialog");
  bookmarkEditDialog.close();
}
</script>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200");
</style>

<template>
  <div class="main">
    <div class="toolbar">
    <input type="text" class="transparent" v-model="urlBox" placeholder="Type URL"/>
    <a :href="urlBox"><span class="material-symbols-outlined">
link
</span></a>
    <a :href="search"><span class="material-symbols-outlined">
search
</span></a>
    <button v-on:click="addReadingList" :disabled="isReadingListUnpushable"><span class="material-symbols-outlined">
bookmark_add
</span></button>
    <button v-on:click="setBookmark" :disabled="isBookmarkUnpushable"><span class="material-symbols-outlined">
star
</span></button>
  </div>
  <!--
  <div>
    <ReadingList v-for="readingList in readingLists" :key="readingList" :url="readingList" @delete="deleteReadingList(readingList)"></ReadingList>
  </div>
  -->
  <div class="bookmarks">
    <Bookmark class="bookmark" v-for="bookmark in bookmarks" :key="bookmark.text" :name="bookmark.name" :url="bookmark.text" :icon="bookmark.icon" @delete="deleteBookmark(bookmark.text)" @update="openBookmarkDialog(bookmark.text, bookmark.icon, bookmark.name)"></Bookmark>
  </div>
  </div>

  <dialog id="bookmarkEditDialog">
    <p style="margin: 0; margin-bottom: 0.5em; text-align: center;">Edit Bookmark</p>
    <div class="bookmarkEditTypeBox">
      <label>Icon</label>
      <input type="text" class="textField" v-model="newIcon">
      <label>Name</label>
      <input type="text" class="textField" v-model="newName">
      <label>URL</label>
      <input type="text" class="textField" v-model="newUrl">
    </div>
    <div id="buttonRibbon">
      <button @click="deleteBookmark(editingBookmark)" class="deleteButton"><span class="material-symbols-outlined">delete</span></button>
      <div></div>
      <button @click="closeBookmarkDialog" style="text-align: center; margin-top: 0.5em;">OK</button>
    </div>
  </dialog>
</template>
