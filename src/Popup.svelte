<!-- Popup.svelte -->
<script>
    import { createEventDispatcher } from 'svelte';

    export let isOpen = false; // Reactive variable to control the popup visibility

    // Create an event dispatcher
    const dispatch = createEventDispatcher();

    function closePopup() {
        isOpen = false; // Close the popup
        dispatch('close'); // Dispatch 'close' event
    }

    let audio;

  function playMusic() {
    audio.play();
  }

  function pauseMusic() {
    audio.pause();
  }
  function refreshPage() {
    window.location.reload();
  }
</script>

{#if isOpen}
    <div class="popup">
        <!-- Popup content -->
        <div class="popup-content">
            <img src = "./images/deathScreen.png" width="1000">
            <div></div>
            <button on:click={refreshPage} style="display: block; margin: 0 auto;">Play Again</button>
            <div></div><!-- Add your popup content here -->
            
        </div>
        <audio src="./audio/babyBlue.mp3" bind:this={audio}></audio>


    </div>
{/if}

<style>
    /* Style your popup here */
    .popup {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.5);
        display: flex;
        justify-content: center;
        z-index: 999; /* Ensure popup is on top of other content */
        align-items: center;
    }

    .popup-content {
        background-color: white;
        padding: 20px;
        border-radius: 5px;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.5);
    }
</style>
