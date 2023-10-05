<script lang="ts">

    import {createEventDispatcher} from "svelte";

    const dispatch = createEventDispatcher();

    function onclick() : void {
        dispatch("click");
    }
</script>

<div class="button" on:click={onclick}>
    <slot/>
</div>

<style lang="scss">
    @import "../global";

    .button {
        @extend %no-sel;
        cursor: pointer;

        width: fit-content;
        border: 1px solid gray; border-radius: 0.3rem;
        padding: 1rem 1.4rem;

        color: #333;

        &:hover {
            background: #eee;
            color: black;
        }

        &:active {
            background: #ddd;
        }
    }
</style>
