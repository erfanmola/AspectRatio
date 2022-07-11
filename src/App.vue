<script setup lang="ts">
import { ref } from 'vue';
import { computed } from '@vue/reactivity';

  let x1:any = ref(1920);
  let y1:any = ref(1080);
  let x2:any = ref(null);
  let y2:any = ref(null);

  const gcd:any = (a:any, b:any) => { if (!(b)) { return a }else{ return gcd(b, a % b); } };

  const aspect = computed(() => {

    if (x1 && y1) {

      return `${ (x1.value / gcd(x1.value, y1.value)) } / ${ y1.value / gcd(x1.value, y1.value) }`;

    }else if (x2 && y2) {

      return `${ (x2.value / gcd(x2.value, y2.value)) } / ${ y2.value / gcd(x2.value, y2.value) }`;

    }else {

      return 'Nothing';

    }

  });

  const Input = (from:any) => {

    if (from === 'x1' && x1.value.toString().length === 0) {
      
      return false;

    }

    if (from === 'x2' && x2.value.toString().length === 0) {
      
      return false;

    }

    if (from === 'y1' && y1.value.toString().length === 0) {
      
      return false;

    }

    if (from === 'y2' && y2.value.toString().length === 0) {
      
      return false;

    }

    let target = null;

    if ((!(x1.value) || x1.value.toString().length < 1) && (y1.value && x2.value && y2.value)) {
    
      target = 'x1';

    }else if ((!(y1.value) || y1.value.toString().length < 1) && (x1.value && x2.value && y2.value)) {

      target = 'y1';

    }else if ((!(x2.value) || x2.value.toString().length < 1) && (x1.value && y1.value && y2.value)) {

      target = 'x2';

    }else if ((!(y2.value) || y2.value.toString().length < 1) && (x1.value && x2.value && y1.value)) {

      target = 'y2';

    }else if ((x1.value && x2.value && y1.value && y2.value) && ((x1.value.toString().length > 0) && (x2.value.toString().length > 0) && (y1.value.toString().length > 0) && (y2.value.toString().length > 0))) {

      switch (from) {

        case 'x1':

          target = 'y1';
          break;

        case 'y1':

          target = 'x1';
          break;

        case 'x2':

          target = 'y2';
          break;

        case 'y2':

          target = 'x2';
          break;

      }
      
    }

    switch (target) {

      case 'x1':
        
        x1.value = (x2.value * y1.value) / y2.value;
        break;

      case 'y1':
        
        y1.value = (x1.value * y2.value) / x2.value;
        break;

      case 'x2':
        
        x2.value = (x1.value * y2.value) / y1.value;
        break;

      case 'y2':
        
        y2.value = (x2.value * y1.value) / x1.value;
        break;

    }

  };

  const Reset = () => {

    x1.value = 1920;
    y1.value = 1080;

    x2.value = null;
    y2.value = null;

  };

</script>

<template>

  <div id="container">
    
    <h1>Aspect Ratio Calculator</h1>

    <section>

      <div>

        <div>
          <input type="number" placeholder="Width A"  v-model="x1" @input="Input('x1')" />
          <input type="number" placeholder="Height A" v-model="y1" @input="Input('y1')" />
        </div>

        <div>
          <input type="number" placeholder="Width B"  v-model="x2" @input="Input('x2')" />
          <input type="number" placeholder="Height B" v-model="y2" @input="Input('y2')" />
        </div>

      </div>

      <span>Aspect Ratio is : <b>{{ aspect }}</b></span>

      <button @click="Reset">Reset</button>

    </section>

  </div>

</template>

<style lang="scss">
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  div#app {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100vh;
  }

  div#container {
    user-select: none;
    padding: 16px;

    > h1 {
      padding-bottom: 32px;
      text-align: center;
    }

    > section {
      display: grid;
      
      > div {
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-gap: 24px;

        > div {
          display: grid;
          grid-gap: 12px;

          > input {
            font-size: 1rem;
            padding: 6px;
            text-align: center;
            width: 100%;
          }
        }
      }

      > span {
        text-align: center;
        padding-top: 32px;
        font-size: 1.125rem;
      }

      > button {
        margin-top: 24px;
        font-size: 1rem;
        font-weight: bold;
        padding: 8px 0;
        cursor: pointer;
      }

    }

  }

</style>