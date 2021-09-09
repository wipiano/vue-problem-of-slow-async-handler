<template>
  <div class="home">
    <div class="box">
      <p>dropdown の選択により，選択した秒数だけ遅れて下のテキストが更新される</p>
      <p>cancellation 機構を持たないため， 10 sec -> 5 sec のように連続で選択すると，最終的な状態が「ドロップダウン: 5 sec, テキスト: 10 sec」となってしまう．</p>
      <select name="data" @change="handleDatasetChanged">
        <option value="0">0 sec</option>
        <option value="5">5 sec</option>
        <option value="10">10 sec</option>
      </select>
      <div class="result">{{ this.text }}</div>
    </div>
    <div class="box">
      <p>dropdown の選択により，選択した秒数だけ遅れて下のテキストが更新される</p>
      <p>簡易的な cancellation 機構を持つので，状態の不整合が起きることはない．</p>
      <select name="data2" @change="handleDatasetChangedWithCancellation">
        <option value="0">0 sec</option>
        <option value="5">5 sec</option>
        <option value="10">10 sec</option>
      </select>
      <div class="result">{{ this.text2 }}</div>
    </div>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src

@Options({
  components: {
    HelloWorld,
  },
})
export default class Home extends Vue {
  text = "0 sec";
  text2 = "0 sec";
  currentPromise: Promise<void> | null = null;

  async handleDatasetChanged(e: any) : Promise<void> {
    const index: number = e.target.selectedIndex
    const sec = index * 5;
    console.log(`${sec} sec : start`)
    await this.sleep(e.target.selectedIndex * 5)
    this.text = `${sec} sec`
    console.log(`${sec} sec : end`)
  }

  async handleDatasetChangedWithCancellation(e: any) {
    const index: number = e.target.selectedIndex
    const sec = index * 5;
    console.log(`${sec} sec : start`)

    // 現在の promise を保存
    const promise = this.sleep(e.target.selectedIndex)
    this.currentPromise = promise;
    await promise;

    // promise が上書きされている場合は，後続の処理を実行しない（疑似 cancellation）
    if (promise === this.currentPromise) {
      this.text2 = `${sec} sec`
      console.log(`${sec} sec : end`)
    } else {
      console.log(`${sec} sec: cancelled!!`)
    }
  }

  async sleep(sec: number): Promise<void> {
    await new Promise((resolve) => setTimeout(resolve, sec * 1000));
  }
}
</script>

<style>
.box {
  margin: 32px;
  border: solid 1px black;
  max-width: 750px;
  margin-left: auto;
  margin-right: auto;
}

select {
  font-size: 16px;
}

.result {
  margin:32px;
  font-size: 20px;
}
</style>
