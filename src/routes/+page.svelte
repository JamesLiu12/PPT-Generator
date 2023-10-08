<script lang="ts">
    import "../global.scss";
    import EditTeachingMaterial from "./EditTeachingMaterial.svelte";
    import EditOutline from "./EditOutline.svelte";
    import EditContent from "./EditContent.svelte";

    const title = "Teaching Material Generator";

    let teachingMaterial: HTMLElement;
    let outline: string = "outline to show";
    let content: string = "content to show";
    const httpAddr = "http://localhost:5173/api/0";

    enum Progress {
        EditingTeachingMaterial,
        EditingOutline,
        EditingPptContent,
    }

    let currentPage: Progress = Progress.EditingTeachingMaterial;

    async function generateOutline(sections: Array<string>) {
        console.log("Generate outline");

        if (sections.length == 0) {
            alert("Teaching material cannot be empty!");
            return;
        }

        const jsonToSend = JSON.stringify({
            "operation": "generate outline",
            "content": sections,
        });
        console.log(jsonToSend)

        const response = await fetch(httpAddr, {
            method: 'POST',
            headers: new Headers({
                "Content-Type": "text/plain",
                "Content-Length": jsonToSend.length.toString(),
            }),
            body: jsonToSend,
        });

        outline = await response.json();
        console.log(outline)

        currentPage = Progress.EditingOutline;
    }

    async function regenerateOutline() {
        const jsonToSend = JSON.stringify({
            "operation": "regenerate outline",
        });

        const response = await fetch(httpAddr, {
            method: 'POST',
            headers: new Headers({
                "Content-Type": "text/plain",
                "Content-Length": jsonToSend.length.toString(),
            }),
            body: jsonToSend,
        });

        outline = await response.json();
        console.log(outline)
    }

    async function generateContent(outline: string) {
        console.log("Generate content");

        if (outline.length == 0) {
            alert("Outline cannot be empty!");
            return;
        }

        const jsonToSend = JSON.stringify({
            "operation": "generate content",
            "content": outline,
        });
        console.log(jsonToSend)

        const response = await fetch(httpAddr, {
            method: 'POST',
            headers: new Headers({
                "Content-Type": "text/plain",
                "Content-Length": jsonToSend.length.toString(),
            }),
            body: jsonToSend,
        });

        content = await response.json();
        console.log(content);

        currentPage = Progress.EditingPptContent;
    }

    async function regenerateContent() {
        const jsonToSend = JSON.stringify({
            "operation": "regenerate content",
        });

        const response = await fetch(httpAddr, {
            method: 'POST',
            headers: new Headers({
                "Content-Type": "text/plain",
                "Content-Length": jsonToSend.length.toString(),
            }),
            body: jsonToSend,
        });

        content = await response.json();
        console.log(content)
    }

    function uploadContent(content: string) {
        console.log("Upload content");

        if (content.length == 0) {
            alert("Content cannot be empty!");
            return;
        }

        const jsonToSend = JSON.stringify({
            "operation": "upload content",
            "content": content,
        });
        console.log(jsonToSend)

        fetch(httpAddr, {
            method: 'POST',
            headers: new Headers({
                "Content-Type": "text/plain",
                "Content-Length": jsonToSend.length.toString(),
            }),
            body: jsonToSend,
        });
    }

    function generatePPT() {
        const jsonToSend = JSON.stringify({
            "operation": "generate ppt",
        });

        fetch(httpAddr, {
            method: 'POST',
            headers: new Headers({
                "Content-Type": "text/plain",
                "Content-Length": jsonToSend.length.toString(),
            }),
        })
            .then(response => response.blob())
            .then((blob) => {
                let url: string = window.URL.createObjectURL(blob);
                let fileLink: HTMLAnchorElement = document.createElement('a');
                fileLink.href = url;
                fileLink.download = 'generated.ppt';
                document.body.appendChild(fileLink);
                fileLink.click();
                fileLink.remove();
            })
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
        <EditOutline on:generate-content={e => generateContent(e.detail)} on:regenerate-outline={e => regenerateOutline()} outlineInput={outline}/>
    {:else}
        <EditContent on:regenerate-content={e => regenerateContent()} on:upload-content={e => uploadContent(e.detail)} on:generate-ppt={e => generatePPT()} contentInput={content} />
    {/if}
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