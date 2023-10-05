<script lang="ts">
    import "../global.scss";
    import EditTeachingMaterial from "./EditTeachingMaterial.svelte";
    import EditOutline from "./EditOutline.svelte";

    const title = "Teaching Material Generator";

    let teachingMaterial: HTMLElement;
    let outline: string = "outline to show";

    enum Progress {
        EditingTeachingMaterial,
        EditingOutline,
        EditingPptContent,
    }

    let currentPage: Progress = Progress.EditingTeachingMaterial;

    async function generateOutline(sections: Array<string>) {
        console.log("Generate");
        if (sections.length == 0) {
            alert("Teaching material cannot be empty!");
            return;
        }

        const sectionsRaw = JSON.stringify(sections);

        // const response = await fetch("");
        // const json = await response.json();

        // TODO

        currentPage = Progress.EditingOutline;
    }

    function generateContent(outline: string) {
        // TODO
        currentPage = Progress.EditingPptContent;
    }

    function regenerateContent() {
        // TODO
    }

    function uploadContent() {
        // TODO
    }

    function generatePPT() {
        // TODO
    }
</script>

<svelte:head>
    <title>{title}</title>
</svelte:head>

<nav>{title}</nav>

<div id="wrapper">
    {#if currentPage === Progress.EditingTeachingMaterial}
        <EditTeachingMaterial on:generate-outline={e => generateOutline(e.detail)}/>
    {:else if currentPage === Progress.EditingOutline}
        <EditOutline on:generate-content={e => generateContent(e.detail)}/>
    {/if}

<!--    <div id="operations">-->
<!--        {#if currentPage === Progress.EditingOutline}-->
<!--            <Button on:click={regenerateOutline}>-->
<!--                Regenerate outline-->
<!--            </Button>-->
<!--            <Button on:click={generateContent}>-->
<!--                Generate content-->
<!--            </Button>-->
<!--        {:else}-->
<!--            <Button on:click={regenerateContent}>-->
<!--                Regenerate content-->
<!--            </Button>-->
<!--            <Button on:click={uploadContent}>-->
<!--                Upload content-->
<!--            </Button>-->
<!--            <Button on:click={generatePPT}>-->
<!--                Generate PPT-->
<!--            </Button>-->
<!--        {/if}-->
<!--    </div>-->
</div>


<style lang="scss">
    nav {
        padding: 1rem 2rem;
        background: #eee;
    }

    #wrapper {
        max-width: 40rem; width: 100%;
        margin: 0 auto;
    }

    :global(.operations) {
        display: flex;
        justify-content: space-around;
    }
</style>