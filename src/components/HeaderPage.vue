<template>
  <div class="header">
    <nav >
      <ul>
        <li></li>
        <li>HOME</li>
        <li>TV POKÉMON</li>
        <li>APP</li>
      </ul>
    </nav>
    <div class="boxHeader">
      <div>
        <h1>Welcome to the world of Pokémon</h1>
        <h4>Where bonds are forged, battles are fought, and friendships last forever.</h4>
        <h3>Let this journey begin</h3>
      </div>
      <img src="../assets/pokebola.png" alt="Imagem Pokebola"/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HeaderPage'
}
</script>

<style scoped>
.header{
  height:100vh;
  margin:0px;
}
.header > nav{
  height:7%;
  background-color: rgba(13, 12, 12, 0.62);
  width:100%;
  margin:0px;
}
.header > nav > ul{
  margin:0px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: end;
  margin-right:6%;
  height:99%;
  gap:1.7rem;
  position:relative;
}
.header > nav > ul > li{
  list-style: none;
  color: #FFFFFF;
  font-family: 'Hind Siliguri', sans-serif;
  font-weight:400;

}
.header > nav > ul > li:nth-child(2){
  color:#0540D9;
}
.header > nav > ul > li:nth-child(1){
  background-image: url("../assets/pokemon-logo-png-1421.png");
  background-size:contain;
  background-position: center top;
  background-repeat: no-repeat;
  width: 10%;
  max-width:137px;
  max-height:105px;
  height:200%;
  position:absolute;
  left:5%;
}
.boxHeader{
  width:80%;
  height:90%;
  display:flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin:auto;

}
.boxHeader > img{
  width:45%;
  height:90%;
  max-width:520px;
  max-height:615px;
  position: relative;
  animation: moverUpDown 4s infinite;
}
.boxHeader > div{
  width:50%;
  height:90%;
  display:flex;
  flex-direction: column;
  justify-content: center;
}
.boxHeader > div > h1, h4, h3{
  margin:0px;
  width:100%;
}
.boxHeader > div > h1{
  font-size:4.4rem;
  color:#E8E8E8;
  font-family: 'Joti One', cursive;
  
}
.boxHeader > div > h4{
  font-size:1.6rem;
  margin-top:13%;
  margin-bottom:3%;
  color:#EAE4E4;
  font-family: 'Hind Siliguri', sans-serif;
  font-weight:400;
}
.boxHeader > div > h3{
  font-size:2.7rem;
  color:#0540D9;
  font-family: 'Joti One', cursive;
}
@keyframes moverUpDown {
  0%, 100% {
    top: 0; 
  }
  50% {
    top: 2rem;
  }
}
@media screen and (min-width:750px) and (max-width:1100px) {
  .header > nav > ul > li{
    font-size:0.9rem;

  }
  .boxHeader > div > h1{
    font-size:3.2rem;
  
  }
  .boxHeader > div > h4{
    font-size:1.1rem;
  }
  .boxHeader > div > h3{
    font-size:2rem;
  }
}
@media screen and (max-width:750px) {
  .boxHeader{
  width:95%;
  height:90%;
  flex-direction: column-reverse;

}
.boxHeader > img{
  width:90%;
  height:60%;

}
.boxHeader > div{
  width:95%;
  height:50%;
  margin-top:-25%;
}
.boxHeader > div > h1, h4, h3{
  text-align: center;
}
  .header > nav > ul > li:not(:first-child){
    display:none;

  }
  .header > nav > ul > li:nth-child(1){
  width: 40%;
  height:150%;
}
  .boxHeader > div > h1{
    font-size:2.2rem;
  
  }
  .boxHeader > div > h4{
    font-size:1rem;
  }
  .boxHeader > div > h3{
    font-size:1.5rem;
  }
}
</style>
