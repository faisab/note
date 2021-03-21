<template>
<main>
  <span><p>notebook</p></span>
</main>
</template>

<script>
export default {
        name: 'dynamictitle'
      }
</script>

<style scoped> 
    * {
  padding: 1%;
  margin-top: 2px;
  margin-bottom: 2px;
  box-sizing: border-box;

  background: #5c8d89;
}

html,
body {
  font-family: 'Fira Code', monospace;
  width: 100%;
  height: 100%;
  font-size: 16px;
}

main {
  display: flex;
  height: 100%;
  align-items: center;
  justify-content: center;
}

p {
  cursor: pointer;
  position: relative;
  display: inline-block;
  font-size: 5rem;
  background: linear-gradient(to bottom, #000, #000 60%, #fff 60%, #fff 100%);
  background-clip: text;
  -webkit-background-clip: text;
  color: transparent;
  background-repeat: no-repeat;
  transition: background 0.2s ease-out;
  white-space: nowrap;
}

span {
  position: relative;
}

span:before {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  height: 26px;
  background: #000;
  bottom: 9px;
  transition: all 0.2s ease-out;
}

p:hover {
  background-position: 0 2px;
}

span:hover:before {
  transform: translateY(4px)
}
</style>

Credit:
Marwan Zibaoui
February 15, 2020
https://codepen.io/RickyMarou/pen/dyoMXYR