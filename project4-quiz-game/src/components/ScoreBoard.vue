<template>
  <div>
      <section class="score">
          Player <span>{{winCount}}</span> x <span>{{loseCount}}</span> Computer
      </section>
  </div>
</template>

<script>
export default {
 props: ['winCount', 'loseCount']
}
</script>

<style >
 
    #app section.score {
        border-bottom: 1px solid black;
        padding: 24px;
        font-size: 18px ;
    }

    #app span{
        padding: 8px;
        font-weight: bold;
        border: 1px solid black;
    }



</style>