<script>

    let date_from = $state("");
    let date_to = $state("");
    let rooms = $state([]);
    let error = $state("");

    async function checkRooms() {
        console.log("เริ่มค้นหา...");
        console.log("date_from:", date_from);
        console.log("date_to:", date_to);
        error = "";
        rooms = [];
        if (!date_from || ! date_to) {
            error = "กรุณาเลือกวันที่";
            console.log("ไม่ได้เลือกวันที่");
            return;
        }

        try {
            const apiurl = `https://booking-api-8ux7.onrender.com/rooms?date_from=${date_from}&date_to=${date_to}`;
            

             console.log("URL:", apiurl);
             const res = await fetch(apiurl);
             console.log("Status:", res.status);

            if (!res.ok) {
                const text = await res.text();
                error = `Error ${res.status}: ${text}`;
                console.log("Error:", error);
                return;
            }
            rooms = await res.json();
            error = "";

            console.log("ได้ข้อมูล:", rooms);
        } catch (e) {
            error = "ไม่สามารถเชื่อมต่อ API ได้: " + e.message;
            console.log("Exception:", e.message);
        }   
}

</script>

<h1> ตรวจสอบห้องว่าง</h1>

<label for= "checkin" >เช็คอิน</label>
<input id="checkin" type="date" bind:value={date_from}> 

<label for= "checkouะ">เช็คเอาท์</label>
<input id="checkout" type="date" bind:value={date_to}>
<button onclick={checkRooms}>ค้นหา</button>

{#if error}
  <p style="color: red;">{error}</p>
{/if}

{#each rooms as room }
    <div>
        ห้อง {room.room_id} - {room.status === "available" ? "✓ ว่าง" : "✗ ไม่ว่าง"}
    </div>
{/each}