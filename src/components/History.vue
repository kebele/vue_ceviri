<template>
    <div class="well">
                <h3>Arama Geçmişi</h3>
                 <ul class="list-group text-left">
                 <li class="list-group-item" v-for="item in history" :key="item.id"> 
                   <div>
                     <strong>{{item.fromLang}} => {{item.toLang}}</strong>
                   </div>
                   <br>
                   <div>
                     <strong class="text-success">{{item.fromText}} => {{item.toText}}</strong> 
                     <span class="text-info"></span>
                   </div>
                 </li>
               </ul>
              </div>
</template>

<script>
    export default {
        props : ["history"]
        //buraya history adında bir props gelecek demek bu gelecek olan şey bir array, bunu her birini li olarak getirsin bunu v-for ile yapacak
        //
    }
</script>

<style>
    .list-group-item{
        margin-bottom : 10px
    }
</style>