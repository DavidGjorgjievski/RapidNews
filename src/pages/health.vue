<template>
  <div :class="darkMode ? 'bodydark' : '' ">
    <div :class="darkMode ? 'sub-header-dark' : 'sub-header'">
         <h1 :class="darkMode ? 'sub-header-title-dark' : 'sub-header-title'">Health</h1>
       <ul class="header-options">
          <li>
            <select class="select" v-model="selectedLanguage">
              <option value="en">English</option>
              <option value="zh-Hans">简体中文</option>
              <option value="zh-Hant">繁體中文</option>
              <option value="id">Bahasa Indonesia</option>
              <option value="cs">Čeština</option>
              <option value="uk">Українська</option>
              <option value="he">עברית</option>
              <option value="ar">العربية</option>
              <option value="de">Deutsch</option>
              <option value="es-419">Español</option>
              <option value="ja">日本語</option>
              <option value="ko">한국어</option>
              <option value="fr">Français</option>
              <option value="it">Italiano</option>
              <option value="lv">Latviešu</option>
              <option value="lt">Lietuvių</option>
              <option value="ml">മലയാളം</option>
              <option value="th">ไทย</option>
              <option value="hu">Magyar</option>
              <option value="nl">Nederlands</option>
              <option value="no">Norsk</option>
              <option value="pl">Polski</option>
              <option value="pt-419">Português  </option>
              <option value="pt-150">Português (Europeu)</option>
              <option value="ro">Română</option>
              <option value="sk">Slovenčina</option>
              <option value="sl">Slovenščina</option>
              <option value="sv">Svenska</option>
              <option value="vi">Tiếng Việt</option>
              <option value="tr">Türkçe</option>
              <option value="el">Ελληνικά</option>
              <option value="bg">Български</option>
              <option value="ru">Русский</option>
              <option value="sr">Српски</option>
              <option value="mr">मराठी</option>
              <option value="hi">हिन्दी</option>
              <option value="bn">বাংলা</option>
              <option value="ta">தமிழ்</option>
              <option value="te">తెలుగు</option>   
            </select>
          </li>
          <li>
            <select class="select" v-model="selectedSort">
                <option value="timestamp">Timestamp</option>
                <option value="title">Title</option>
            </select>
          </li>
        <li>
            <div class="dark-mode-div">
            <input type="checkbox" id="darkmode-toggle"  v-model="darkMode"/>
            <label for="darkmode-toggle"/>
            </div>
        </li>
        </ul>
    </div>
      <hr class="lightBlueLine">
      <div :class="darkMode ? 'sectionDiv-dark' : 'sectionDiv'">
        <Section :selectedLanguage="selectedLanguage" :selectedSort="selectedSort" :darkMode="darkMode" :category="category" />
      </div>
  </div>  
</template>

<script>

import Section from '@/components/Section.vue';

export default {
    data(){
        return {
            selectedLanguage: 'en',
            selectedSort: 'timestamp',
            darkMode: false,
            category: 'health',
        }
    },
    components: {
        Section,
    },

}
</script>

<style scoped>
h2 {
  margin-bottom: 20px;
  font-size: 36px;
}

p {
  margin: 20px 0;
}

.bodydark{
  background: #011627;
}

.sub-header{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 20px;
}

.sub-header-dark{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 20px;
    background: #011627;
}

.sub-header-title{
    color: black;
    font-size: 1.7rem;
    font-weight: bold;
}

.sub-header-title-dark{
    color: white;
    font-size: 1.7rem;
    font-weight: bold;
}

.sectionDiv{

}

.sectionDiv-dark{
    background: #011627;
}

.header-options {
  display: flex;
  flex-direction: row;
}

.header-options li {
  color: black;
  list-style-type: none;
  margin-right: 20px;
}

.select {
  background-color: #ebebeb;
  color: black;
  padding: 8px;
  border: none;
  border-radius: 5px;
  font-size: 14px;
  outline: none;
  cursor: pointer;
}

.select:hover {
  background-color: #ebebeb;
}

.select option {
  background-color: #ebebeb;
  color: black;
}

/* toggle switch */

label {
  width:50px;
  height:20px;
  position: relative;
  display: block;
  background: #ebebeb;
  border-radius: 200px;
  box-shadow: inset 0px 5px 15px rgba(0,0,0,0.4), inset 0px -5px 15px rgba(255,255,255,0.4);
  cursor: pointer;
}

label:after {
  content: "";
  width:18px;
  height: 18px;
  position: absolute;
  top:1px;
  left:1px;
  background: linear-gradient(180deg,#ffcc89,#d8860b);
  border-radius: 180px;
  box-shadow: 0px 5px 10px rgba(0,0,0,0.2);
  transition: 0.3s;
}
input {
  width: 0;
  height: 0;
  visibility: hidden;
}

input:checked + label {
  background: #242424;
}
input:checked + label:after {
  left:49px;
  transform: translateX(-100%);
  background: linear-gradient(180deg,#777,#3a3a3a);
}
label, label:after {
  transition: 0.3s
}

label:active:after{ 
  width: 26px; 
}

.dark-mode-div{
  margin-top: -20px;
}

.lightBlueLine {
  color: #669bbc;
  height: 2px;
  background: #669bbc;
  margin: 0;
  border: #669bbc;
  padding: 0;
}


@media (max-width: 600px) {
 .sub-header-title,.sub-header-title-dark{
  font-size: 1rem;
  margin-right: 10px;
 }

 .sub-header,.sub-header-dark{
  padding-left: 4px;
 }

 .select{
  padding: 4px;
  font-size: 10px;
 }
}
</style>
