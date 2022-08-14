<template>
   <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTXqTSL7kn-ARoKoXRqRTD6nP7SjvGeaeCH1Q&usqp=CAU" 
      alt="img_data" 
      width="80" />

   <div id="nav">
      <router-link to="/">Inicio</router-link>
   </div>
   <router-view></router-view>

</template>

<script>
   export default {
      name: "App"
   };
</script>