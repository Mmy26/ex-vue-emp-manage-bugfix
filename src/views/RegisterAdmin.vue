<template>
  <div class="container">
    <div class="row register-page">
      <form class="col s12" id="reg-form">
        <div class="row">
          <div>{{ lastNameError }}</div>
          <div class="input-field col s6">
            <input
              id="last_name"
              type="text"
              class="validate"
              v-model="lastName"
              required
            />
            <label for="last_name">姓</label>
          </div>
          <div>{{ firstNameError }}</div>
          <div class="input-field col s6">
            <input
              id="first_name"
              type="text"
              class="validate"
              v-model="firstName"
              required
            />
            <label for="first_name">名</label>
          </div>
        </div>
        <div>{{ addressError }}</div>
        <div class="row">
          <div class="input-field col s12">
            <input
              id="email"
              type="email"
              class="validate"
              v-model="mailAddress"
              required
            />
            <label for="email">メールアドレス</label>
          </div>
        </div>
        <div>{{ passwordError }}</div>
        <div class="row">
          <div class="input-field col s12">
            <input
              id="password"
              type="password"
              class="validate"
              minlength="8"
              v-model="password"
              required
            />
            <label for="password">パスワード</label>
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input
              id="passwordConfirm"
              type="password"
              class="validate"
              minlength="8"
              v-model="passwordConfirm"
              required
            />
            <label for="passwordConfirm">確認用パスワード</label>
          </div>
        </div>
        <div class="row">
          <div class="input-field col s6">
            <button
              class="btn btn-large btn-register waves-effect waves-light"
              type="button"
              v-on:click="registerAdmin"
            >
              登録
              <i class="material-icons right">done</i>
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import config from "@/const/const";
import axios from "axios";

/**
 * 管理者登録をする画面.
 */
@Component
export default class RegisterAdmin extends Vue {
  // 姓
  private lastName = "";
  // 名
  private firstName = "";
  // メールアドレス
  private mailAddress = "";
  // パスワード
  private password = "";
  // 確認用パスワード
  private passwordConfirm = "";
  // エラーメッセージ
  private errorMessage = "";
  // 姓のエラーメッセージ
  private lastNameError = "";
  // 名のエラーメッセージ
  private firstNameError = "";
  // メールアドレスのエラーメッセージ
  private addressError = "";
  // パスワードのエラーメッセージ
  private passwordError = "";
  // フォームチェッカー
  private formChecker = true;

  /**
   * 管理者情報を登録する.
   *
   * @remarks
   * 本メソッドは非同期でWebAPIを呼び出し管理者登録をするためasync/await axiosを利用しています。これらを利用する場合は明示的に戻り値にPromiseオブジェクト型を指定する必要があります。
   * @returns Promiseオブジェクト
   */
  async registerAdmin(): Promise<void> {
    // 管理者登録処理
    const response = await axios.post(`${config.EMP_WEBAPI_URL}/insert`, {
      name: this.lastName + " " + this.firstName,
      mailAddress: this.mailAddress,
      password: this.password,
    });
    console.dir("response:" + JSON.stringify(response));


    if (this.lastName === "") {
      this.firstNameError = "名が入力されていません";
      this.formChecker = false;
    }
    if (this.firstName === "") {
      this.lastNameError = "姓が入力されていません";
      this.formChecker = false;
    }
    if (this.mailAddress === "") {
      this.addressError = "メールアドレスが入力されていません";
      this.formChecker = false;
    }
    if (this.password === "") {
      this.passwordError = "パスワードが入力されていません";
      this.formChecker = false;
    }
    if (this.password != this.passwordConfirm) {
      this.errorMessage = "パスワードが一致しません";
      this.formChecker = false;
      
    }
    
    if (this.formChecker === false) {
      return
    }
    
    if (response.data.status === "success") {
      this.$router.push("/loginAdmin");
    } else {
      this.errorMessage = "登録が失敗しました";
    }
    


  }
}
</script>

<style scoped>
.register-page {
  width: 600px;
}
</style>
