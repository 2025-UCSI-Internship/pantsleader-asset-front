<script>
  import { user } from '$lib/stores/user';
  import { page } from '$app/stores';
  import { onDestroy } from 'svelte';

  let currentUser;
  const unsubscribe = user.subscribe(value => {
    currentUser = value;
  });
  onDestroy(unsubscribe);

  function logout() {
    user.set(null);
    window.location.href = '/login';
  }
</script>

{#if $page.url.pathname !== '/login' && $page.url.pathname !== '/signup'}
<header style="display:flex; justify-content:space-between; padding:10px; background:#333; color:white;">
  <h1 style="margin:0;">DS Asset</h1>
  {#if currentUser}
    <div>
      <span>{currentUser.id}님</span>
      <button on:click={logout} style="margin-left:10px;">로그아웃</button>
    </div>
  {:else}
    <a href="/login" style="color:white;">로그인</a>
  {/if}
</header>
{/if}

<main style="padding:20px;">
  <slot /> <!-- 페이지 콘텐츠 들어감 -->
</main>
