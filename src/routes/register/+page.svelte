<script>
    let username = $state("");
    let password = $state("");
    let error = $state("");
    let success = $state("");
    
     async function doRegister() {
        error = "";    
        success = "";  

        if (!username || ! password) {
            error = "กรุณากรอก username และ password";
            return;
        }
         try {
             const apiurl = `https://booking-api-8ux7.onrender.com/register`;
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
             success = "สมัครสมาชิกสำเร็จ! ไปที่หน้า login เพื่อเข้าสู่ระบบ";


         } catch(e) {
            error = "ไม่สามารถเชื่อมต่อ API ได้!: " + e.message 
         }
    }

</script>

<h1>สมัครสมาชิก</h1>
<input type="text" placeholder="Username" bind:value={username}>
<input type="password" placeholder="Password" bind:value={password}>
<button onclick={doRegister}>สมัครสมาชิก</button>

{#if error}
 <p style="color: red;">{error}</p>
{/if}

{#if success}
<p style="color: green;">{success}</p>


<a href="/login">ไปที่หน้าเข้าสู่ระบบ</a>
{/if}