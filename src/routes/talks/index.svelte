<script>
	import { setClient, operationStore, query } from '@urql/svelte';
	import client from '../../client';

	setClient(client);

	const talks = operationStore(
		`
        query{
            talks{
                data{
                    _id
                    description
                    title
                    video_url
                    speaker{
                        firstname
                        lastname
                        emai
                        pincture
                        bio
                    }
                }
            }
        }
        `
	);
	query(talks);
</script>

<h1>Todos Hablando</h1>
{#if $talks.fetching}
	<p>Loading..</p>
{:else if $talks.error}
	<p>{$talks.error.message}</p>
{:else}
	<ul>
		{#each $talks.data.talks.data as talk}
			<li>
				<h2>{talk.title}</h2>
				<p>{talk.description}</p>
				<iframe
					title="Fauna con Svelte"
					width="560"
					height="315"
                    src={`https://www.youtube.com/embed/${talk.video_url}`}
				/>
			</li>
			
			<li>
				<h1>Persona</h1>
				<h2>{talk.speaker.lastname} {talk.speaker.firstname}</h2>
				<p>{talk.speaker.emai}</p>
				<p>{talk.speaker.pincture}</p>
				<p>{talk.speaker.bio}</p>
			</li>
		{/each}
	</ul>
{/if}
