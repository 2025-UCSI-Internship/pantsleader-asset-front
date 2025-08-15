<script>
  import { goto } from '$app/navigation';

  let id = '';
  let password = '';
  let confirmPassword = '';
  let email = '';

  function signup() {
    if (!id || !password || !confirmPassword || !email) {
      alert('모든 필드를 입력해주세요.');
      return;
    }
    if (password !== confirmPassword) {
      alert('비밀번호가 일치하지 않습니다.');
      return;
    }
    alert('회원가입 성공!');
    goto('/login');
  }
</script>

<div class="register-container">
  <h1>회원가입</h1>
  <div class="form-group">
    <input type="text" placeholder="아이디" bind:value={id} />
  </div>
  <div class="form-group">
    <input type="email" placeholder="이메일" bind:value={email} />
  </div>
  <div class="form-group">
    <input type="password" placeholder="비밀번호" bind:value={password} />
  </div>
  <div class="form-group">
    <input type="password" placeholder="비밀번호 확인" bind:value={confirmPassword} />
  </div>

  <button on:click={signup}>가입하기</button>
  <p class="back-login">
    이미 계정이 있으신가요?
    <a href="/login">로그인</a>
  </p>
</div>

<style>
  /* 카드 스타일 */
  .register-container {
    width: 100%;
    max-width: 420px;
    background-color: rgba(27, 38, 59, 0.95);
    padding: 40px 35px;
    border-radius: 16px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.25);
    text-align: center;
    animation: fadeIn 0.4s ease-in-out;
  }

  h1 {
    margin-bottom: 25px;
    color: #e0e1dd;
    font-size: 26px;
    font-weight: 700;
  }

  .form-group {
    margin-bottom: 15px;
  }

  input {
    width: 100%;
    padding: 14px;
    border: none;
    border-radius: 8px;
    font-size: 15px;
    background-color: #fff;
    color: #333;
    outline: none;
    transition: all 0.2s;
  }

  input:focus {
    box-shadow: 0 0 0 3px rgba(69, 123, 157, 0.4);
  }

  button {
    width: 100%;
    padding: 14px;
    margin-top: 10px;
    background-color: #415a77;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.2s, transform 0.1s;
  }

  button:hover {
    background-color: #1b263b;
    transform: scale(1.02);
  }

  .back-login {
    margin-top: 20px;
    font-size: 14px;
    color: #adb5bd;
  }

  .back-login a {
    color: #e0e1dd;
    font-weight: 500;
    text-decoration: none;
    transition: color 0.2s;
  }

  .back-login a:hover {
    color: #f8f9fa;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }
</style>
