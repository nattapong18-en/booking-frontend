<script>
    let username = $state("");
    let password = $state("");
    let error = $state("");
    let success = $state("");

    async function doLogin() {
        error = "";    
        success = "";  

        if (!username || ! password) {
            error = "กรุณากรอก username และ password";
            return;
        }
         try {
             const apiurl = `https://booking-api-8ux7.onrender.com/login`;
             const res = await fetch(apiurl, {
                method: "POST",
                headers: {"Content-Type": "application/json"},
                body: JSON.stringify({username, password})
             });
            
             if (!res.ok) {
                const text = await res.text();
                error = `Error ${res.status}: ${text}`;
                return;
             }

             const data = await res.json();
             localStorage.setItem("token", data.token);
             success = "ล็อคอินสำเร็จ!";
 
            setTimeout(() => {
                window.location.href = "/book";
            }, 1000);

         } catch(e) {
            error = "ไม่สามารถเชื่อมต่อ API ได้!: " + e.message 
         }
    }

</script>

<h1>🔐 ล็อคอิน</h1>
<input type="text" placeholder="Username" bind:value={username}>
<input type="password" placeholder="Password" bind:value={password}>

<button onclick={doLogin}>เข้าสู่ระบบ</button>

{#if error}
  <p style="color: red;">{error}</p>
  
{/if}
<a href="/register">ลงทะเบียนใช้งาน</a>
{#if success}

<p style="color: green;">{success}</p>
{/if}