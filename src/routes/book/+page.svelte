<script>
    let room_id = $state("");
    let start_time = $state("");
    let end_time = $state("");
    let error = $state("");
    let success = $state("");

    async function doBooking() {
        error = "";
        success = "";

        const token = localStorage.getItem("token");

        if(!token) {
            error = "กรุณาล็อคอินก่อน";
            return;
        }


        if (!room_id || !start_time || !end_time) {
            error = "กรุณากรอกข้อมูลให้ครบ";
            return;
        }


        try {
            const apiurl = `https://booking-api-8ux7.onrender.com/book`;
            const res = await fetch(apiurl, {
                method: "POST",
                headers: {
                       "Content-Type": "application/json",
                       "Authorization": `Bearer ${token}`   
                },
                body: JSON.stringify({
                              room_id: Number(room_id), 
                              start_time: start_time + ":00Z",
                              end_time: end_time + ":00Z",
                })
             });

            if (!res.ok) {
                const text = await res.text();
                error = `Error ${res.status}: ${text}`;
                return;
            }

             const data = await res.json();
             success = `จองห้อง ${room_id} สำเร็จ!`;
           

        } catch (e) {
            error = "ไม่สามารถเชื่อมต่อ API ได้!:" +e.message;
        }
    }
</script>

<h1>📝 จองห้องพัก</h1>
<input type="number" placeholder="เลขห้อง" bind:value={room_id}>
<input type="datetime-local" placeholder="เช็คอิน" bind:value={start_time}>
<input type="datetime-local" placeholder="เช็คเอาท์" bind:value={end_time}>
<button onclick={doBooking}>จองห้อง</button>



{#if error}

<p style="color: red">{error}</p>
{#if error == "กรุณาล็อคอินก่อน"}
 <a href="/login">ไปหน้าล็อคอิน</a>
{/if}

{/if}

{#if success}
<p style="color: green;">{success}</p>
<a href="/bookings">ดูรายการจอง</a>
{/if} 