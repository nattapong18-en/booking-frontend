<script>
    import { onMount } from "svelte";
    import favicon from '$lib/assets/favicon.svg';
    let { children } = $props();

    let loggedIn = $state(false);

    function checkLogin() {
        loggedIn = !!localStorage.getItem("token");
    }

    function logout() {
        localStorage.removeItem("token");
        loggedIn = false;
        window.location.href = "/";
    }

    onMount(() => {
        checkLogin();
    });
</script>

<svelte:head>
    <link rel="icon" href={favicon} />
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@picocss/pico@2/css/pico.min.css">
</svelte:head>

<nav class="container">
    <ul>
        <li><strong>จองห้องพัก</strong></li>
    </ul>
    <ul>
        <li><a href="/">ห้องว่าง</a></li>
        {#if loggedIn}
            <li><a href="/book">จอง</a></li>
            <li><a href="/bookings">การจอง</a></li>
            <li><button onclick={logout}>ออกจากระบบ</button></li>
        {:else}
            <li><a href="/login">ล็อกอิน</a></li>
            <li><a href="/register">สมัคร</a></li>
        {/if}
    </ul>
</nav>

<main class="container">
    {@render children()}
</main>

