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

    function fileAdded(event: CustomEvent<ReadonlyArray<File>>) {
        const files = event.detail;
        console.log("Files added: ", files);

        for (const file of files) {
            console.log(file.name, file.size);
        }
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
                    on:add={fileAdded}
                />
            </Column>
        </Row>
    </Grid>
</Content>
