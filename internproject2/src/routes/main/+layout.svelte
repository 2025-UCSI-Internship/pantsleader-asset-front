<!-- src/routes/+layout.svelte -->
<script>
  import { page } from '$app/stores';
  $: path = $page.url.pathname;
  const isActive = (href) => path === href || path.startsWith(href + '/');
</script>

<svelte:head>
  <title>DS Asset</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
</svelte:head>

<!-- 라운드/글래스 헤더 -->
<header class="site-header">
  <div class="nav">
    <a class="brand" href="/">DS Asset</a>

    <div class="links">
      <a class:active={isActive('/')} href="/">home</a>
      <a class:active={isActive('/assets')} href="/assets">asset list(자산목록)</a>
      <a class:active={isActive('/assets/new')} href="/assets/new">asset registration(자산등록)</a>
      <a class:active={isActive('/movements')} href="/movements">asset transfer(자산이동)</a>
    </div>
  </div>
</header>

<!-- 페이지 내용 -->
<slot />

<style>
  :root{
    --nav-max: 1080px;
    --ring: rgba(10, 102, 255, .08);
  }

  /* 상단 여백 주고, 헤더 전체를 스티키로 */
  .site-header{
    position: sticky; top: 10px; z-index: 50;
    padding: 0;  /* 좌우 바깥 여백 */

  }

  /* 유리 느낌 + 라운드 + 그림자 */
  .nav{
    max-width: var(--nav-max); margin: 0 auto;
    display:flex; align-items:center; justify-content:space-between; gap:14px;

    padding: 12px 16px;
    border-radius: 18px;
    background:
      linear-gradient(180deg, rgba(255,255,255,.86), rgba(255,255,255,.78));
    border: 1px solid rgba(13, 23, 44, .10);
    box-shadow: 0 10px 30px rgba(16, 30, 55, .15);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
  }

  /* 위쪽만 살짝 하이라이트 링 */
  .nav::before{
    content:""; position:absolute; inset:-2px -2px auto -2px; height:10px;
    border-radius: 20px 20px 0 0; background: linear-gradient(90deg, transparent, var(--ring), transparent);
    pointer-events:none;
  }

  .brand{
    font-weight: 800; letter-spacing:.2px;
    text-decoration:none; color:#0e1116;
  }

  .links{ display:flex; gap:8px; flex-wrap:wrap; }

  .links a{
    text-decoration:none; color:#334155;
    padding: 8px 12px; border-radius: 12px;
    border: 1px solid transparent; transition: all .15s ease;
  }
  .links a:hover{
    background:#f1f5ff; border-color:#e4ecff; color:#0a3cff;
  }
  .links a.active{
    background: linear-gradient(180deg, #eaf1ff, #dfeaff);
    border-color:#d6e4ff; color:#0a66ff; font-weight:600;
    box-shadow: inset 0 1px 0 #fff, 0 0 0 3px rgba(10,102,255,.08);
  }

  /* 작은 화면 */
  @media (max-width: 560px){
    .site-header{ top: 6px; padding: 0 8px; }
    .nav{ padding: 10px 12px; border-radius: 16px; }
    .links{ gap:6px; }
    .links a{ padding:7px 9px; }
  }
</style>
