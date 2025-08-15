<script>
  import { goto } from '$app/navigation';
  import { user } from '$lib/stores/user';

  let ID = "";
  let password = "";

  function login() {
    if (ID.trim() && password.trim()) {
      // 실제론 서버 검증 필요
      user.set({ id: ID });
      goto('/'); // 홈으로 이동
    } else {
      alert('아이디와 비밀번호를 입력하세요.');
    }
  }

  function goSignup() {
    goto('/signup'); // 회원가입 페이지로 이동
  }
</script>

<div class="login-card">
  <h1 class="title">DS Asset</h1>
  <input type="text" placeholder="ID" bind:value={ID} />
  <input type="password" placeholder="Password" bind:value={password} />
  <button class="btn" on:click={login}>SIGN IN</button>
  <button class="btn" on:click={goSignup}>CREATE ACCOUNT</button>
</div>

<style>
  .login-card {
    background: rgba(255, 255, 255, 0.08);
    padding: 50px;
    border-radius: 25px;
    box-shadow: 0 8px 40px rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(12px);
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 550px;
    max-width: 90%;
    margin: 50px auto;
  }

  .title {
    font-size: 2.2rem;
    margin-bottom: 30px;
    color: white;
    font-weight: bold;
  }

  input {
    width: 100%;
    padding: 15px;
    margin: 10px 0;
    border-radius: 12px;
    border: none;
    outline: none;
    background: rgba(255, 255, 255, 0.15);
    color: white;
    font-size: 1.1rem;
  }

  .btn {
    width: 100%;
    padding: 15px;
    margin-top: 10px;
    border-radius: 12px;
    border: none;
    background: #3b82f6;
    color: white;
    font-weight: bold;
    font-size: 1.1rem;
    cursor: pointer;
  }
</style>
