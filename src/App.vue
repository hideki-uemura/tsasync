<template>
  <span>
    <div >
        {{area01}}
    </div>
    <div >
        {{area02}}
    </div>
    <div >
        {{area03}}
    </div>
    <div >
        {{area04}}
    </div>
  </span>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import axios from "axios";

@Component({})
export default class App extends Vue {
  private area01: string = "エリア０１";
  private area02: string = "エリア０２";
  private area03: string = "エリア０３";
  private area04: string = "エリア０４";

  public async mounted() {
    try {
      this.area01 = await this.process01();
    } catch (e) {
      this.area01 =e.message;
    }
    let proc02Flag = false;
    try {
      this.area02 = await this.process02();
      proc02Flag = true;
    } catch (e) {
      this.area02 = e.message;
    }
    try {
      this.area03 = await this.process03(proc02Flag);
    } catch (e) {
      this.area03 = e.message;
    }
    try {
      this.area04 = await this.process04();
    } catch (e) {
      this.area04 = e.message
    }
  }

  //処理０１：５秒まって画面に描画
  protected process01(): Promise<any> {
    return new Promise((resolve: Function, reject: Function) => {
      setTimeout(function() {
        resolve("5秒たちました");
      }, 5000);
    });
  }
  protected async process02(): Promise<string> {
    try{
      await axios.get("https://jenkins.cs-fact.com/demo/test01/");
      return "成功しました１"
    } catch(e){
      throw new Error("なんかエラー");  
    }
  }

protected async process03(proc02Flag: boolean): Promise<any> {
    return new Promise((resolve: Function, reject: Function) => {
      setTimeout(function() {
        resolve("3秒たちました");
      }, 3000);
    });
  }

protected async process04(): Promise<string> {
    try{
      const x  = await axios.get("https://jenkins.cs-fact.com/demo/test02/");
      return "成功しました２"
    } catch(e){
      throw new Error("なんかエラー");  
    }
  }
}
</script>

<style>
div {
  border-width: thin;
  border-style: solid;
  width: 100%;
}
</style>
