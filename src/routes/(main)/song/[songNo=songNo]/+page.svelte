<script lang="ts">
    import TitleDisplay from "$lib/components/page/song/[songNo]/TitleDisplay.svelte";
    import MultipleTitleDisplay from "$lib/components/page/song/[songNo]/MultipleTitleDisplay.svelte";
    import SongDataDisplay from "$lib/components/page/song/[songNo]/SongDataDisplay.svelte";
    import { getIsMobile } from "$lib/module/layout/isMobile";
    import GenreDisplay from "$lib/components/page/song/[songNo]/GenreDisplay.svelte";
    import AlertDisplay from "$lib/components/page/song/[songNo]/AlertDisplay.svelte";
    import CourseContainer from "$lib/components/page/song/[songNo]/CourseContainer.svelte";
    import AddSongButton from "$lib/components/page/song/AddSongButton.svelte";
    import PageTitle from "$lib/components/common/PageTitle.svelte";
    import { getI18N, getLang } from "$lib/module/common/i18n/i18n.js";

    export let data;
    const song = data.song;
    const isMobile = getIsMobile();

    const lang = getLang();
    $: i18n = getI18N('/song/[songNo]', $lang);
    $: titleI18n = getI18N('other', $lang).title['/song/[songNo]'];
</script>

{#if song}
    <PageTitle title={song.title}/>
    <AlertDisplay
        isAsiaBanned={song.isAsiaBanned}
        isKrBanned={song.isKrBanned}
        isDeleted={song.isDeleted}
    />
    <GenreDisplay genres={song.genre} />
    <TitleDisplay title={song.title} songNo={song.songNo} />
    <div class="wrapper" data-isMobile={$isMobile}>
        <MultipleTitleDisplay
            titleKo={song.titleKo}
            titleEn={song.titleEn}
            aliasKo={song.aliasKo}
            aliasEn={song.aliasEn}
        />
        <SongDataDisplay
            bpm={song.bpm}
            bpmShiver={song.bpmShiver}
            version={song.version}
            artists={song.artists}
            addedDate={song.addedDate}
        />
    </div>
    <CourseContainer courses={song.courses} />
{:else}
    <PageTitle title={titleI18n}/>
    {i18n.noSong}
    <AddSongButton />
{/if}

<style>
    .wrapper {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;

        column-gap: 10px;
        row-gap: 10px;
    }

    .wrapper[data-isMobile="true"] {
        flex-direction: column;
    }
</style>
