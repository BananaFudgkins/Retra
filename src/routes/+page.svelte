<script lang="ts">
    import {
        Header,
        HeaderNav,
        HeaderNavItem,
        HeaderNavMenu,
        SkipToContent,
        Content,
        Grid,
        Row,
        Column,
        FileUploader,
    } from "carbon-components-svelte";

    let parsedData: string[] = [];

    async function onAdd(event: CustomEvent<ReadonlyArray<File>>) {
        const files = event.detail;

        for (const file of files) {
            if (file.type === "application/json") {
                let json = await readJson(file);
                parsedData.push(json as string);
            }
        }

        console.log("Parsed", parsedData.length, "files.");
    }

    function readJson(file: File) {
        const reader = new FileReader();
        return new Promise((resolve, reject) => {
            reader.onload = () => resolve(JSON.parse(reader.result as string));
            reader.onerror = reject;
            reader.readAsText(file);
        });
    }
</script>

<Header company="Deja Jackson" platformName="Retra">

</Header>

<Content>
    <Grid>
        <Row>
            <Column>
                <h1>Welcome</h1>
            </Column>
        </Row>
        <Row>
            <Column>
                <FileUploader
                    multiple
                    labelTitle="Upload files"
                    buttonLabel="Add files"
                    labelDescription="Upload your Spotify extended listening history. Only JSON files are accepted."
                    accept={[".json"]}
                    status="complete"
                    on:add={onAdd}
                />
            </Column>
        </Row>
    </Grid>
</Content>
