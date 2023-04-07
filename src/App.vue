<template>
  <div class="container">    
    <main>
    </main>
    <footer>
      <button class="share-payment">分帳款項</button>
      <button class="add-payment">&#x2b;</button>
      <button class="end-share">結束分帳</button>
    </footer>    
  </div>  
</template>

<script setup>  
</script>

<style>  
  .container {
    position: relative;
    display: block;
    width: 300px;
    background-color: var(--main-background);
    height: 600px;
    border-radius: 10px;
    margin: auto;
    overflow: hidden;
  }

  main {
    margin-top: 20px;
    height: 90%;
    overflow: auto;
  }

  main::-webkit-scrollbar {
    width: 7px;
  }

  main::-webkit-scrollbar-button {
    background: transparent;
    border-radius: 4px;
  }

  main::-webkit-scrollbar-track-piece {
    background: transparent;
  }

  main::-webkit-scrollbar-thumb {
    border-radius: 4px;
    background-color: rgba(0, 0, 0, 0.4);
    border: 1px solid slategrey;
  }

  main::-webkit-scrollbar-track {
    box-shadow: transparent;
  }
  
  footer {
    position: absolute;
    display: flex;
    justify-content: space-between;
    background-color: var(--main-background);
    left: 0;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    height: 50px;
    border-top: 2px solid #000;
    z-index: 2;
  }

  footer button{
    cursor: pointer;
    border: none;
    background-color: var(--main-background);
    font-size: 1.2rem;
    font-weight: bold;
  }

  .share-payment,
  .end-share {
    transform: scale(.9);
  }

  .share-payment:hover,
  .end-share:hover{
    transform: scale(1);
  }

  .add-payment {
    position: absolute;
    left: 50%;
    top: 0;
    transform: translate(-50%, -50%) scale(.9);
    background-color: var(--main-background);
    border: 2px solid #000;
    width: 70px;
    height: 70px;
    border-radius: 50%;
    font-size: 3.5rem;
    font-weight: normal;    
  }

  .add-payment:hover {
    transform: translate(-50%, -50%) scale(1);
  }
  
</style>
