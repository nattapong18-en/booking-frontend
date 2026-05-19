<script>
    import { onMount } from "svelte";

    let bookings = $state([]);
    let error = $state("");
    let success = $state("");

    async function loadBookings() {
        error = "";
        success = "";
        
        const token = localStorage.getItem("token");
        if (!token) {
            error = "กรุณาล็อคอิน";
            return;
        }

        try {
             const res = await fetch(`https://booking-api-8ux7.onrender.com/bookings`,{
                headers: {
                    "Authorization": `Bearer ${token}`
                }
             });

             if (!res.ok) {
                const text = await res.text();
                error = `Error ${res.status}: ${text}`;
                return;
             }
             
             bookings = await res.json();

        } catch (e) {
            error = "ไม่สามารถเชื่อมต่อ API ได้:" + e.message;
        }
    }

    async function cancelBooking(id) {
        error = "";
        success = "";
        const token = localStorage.getItem("token");
        const res = await fetch(`https://booking-api-8ux7.onrender.com/cancel/${id}`, {
            method: "PATCH",
            headers: {
                "Authorization": `Bearer ${token}`
            }
        });

        if (res.ok) {
            success = "ยกเลิกสำเร็จ!";
            loadBookings();
        }
    }

    onMount (() => {
        loadBookings();
    });

</script>

<h1>📋 การจองของฉัน</h1>

{#if error}
    <p style="color: red;">{error}</p>
    <a href="/login">กลับไปที่หน้าล็อคอิน</a>
{/if}

{#each bookings as booking }
    <div>
        ห้อง {booking.room_id} - {booking.start_time} ถึง {booking.end_time} - {booking.state}
        {#if booking.state == "Confirmed"}
            <button onclick={() => cancelBooking(booking.booking_id)}>ยกเลิก</button>
        {/if}
    </div>
{/each}
