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

    type SpotifyListen = {
        ts: string;
        ms_played: number;
        track_name: string;
        artist_name: string;
        album_name: string;
        shuffle: boolean;
        skipped: boolean;
    }

    let parsedData: string[] = [];

    function isSpotifyListenArray(data: any): data is SpotifyListen[] {
        return Array.isArray(data) && data.every( item =>
            typeof item.ts === 'string' &&
            typeof item.ms_played === 'number'
        );
    }

    async function onAdd(event: CustomEvent<ReadonlyArray<File>>) {
        const files = event.detail;
        const allListens: SpotifyListen[] = [];

        for (const file of files) {
            if (file.type === "application/json") {
                const text = await file.text();
                let json = JSON.parse(text);
                
                if (isSpotifyListenArray(json)) {
                    allListens.push(...json);
                }
            }
        }

        const grouped: Record<string, SpotifyListen[]> = {};

        allListens.forEach((listen) => {
            const date = new Date(listen.ts).toISOString().split('T')[0];
            if (!grouped[date]) grouped[date] = [];
            grouped[date].push(listen);
        });

        console.log("Parsed", parsedData.length, "files.");
        console.log(parsedData);
    }
</script>

<Header company="Deja Jackson" platformName="Retra"/>

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
