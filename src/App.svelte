<script>
	import {Collapsible, Card, Input} from "spaper";
	let projects = [];
	let filter = "";

	async function loadProjects() {
		projects = await (await fetch("/projects.json")).json();
	}

	loadProjects();
	
	$: filteredProjects = projects.filter(project => project.technologies.join().toLowerCase().includes(filter.toLowerCase())).reverse();
</script>

<main>
	<h2>A Project a Week</h2>
	<h4>An ongoing list of projects created by Thomas Jewitt, limited to the 48 hours of the standard weekend.</h4>

	<div class="row flex-center">
		<Input placeholder="Filter by Technology" 
			bind:value={filter} />
	</div>
	<div class="row flex-center no-bottom">
		<div class="col md-8">
			{#if filteredProjects != undefined}
				{#each filteredProjects as project}
					<div class="collapsible-container">
						<Collapsible label="{project.week}: {project.title}">
							<Card 
								title={project.title}
								subTitle={project.subtitle}
								image={project.image}
							>
								<p>{project.description}</p>
								<div class="row flex-spaces">
									<div class="col md-4">
										<p>Technologies Used</p>
										<ul>
											{#each project.technologies as technology}
											<li>{technology}</li>
											{/each}
										</ul>
									</div>
									<div class="col md-4">
										<p>Learning Outcomes</p>
										<ul>
											{#each project.learning as learning}
											<li>{learning}</li>
											{/each}
										</ul>
									</div>
								</div>
								<a class="card-link" target="_blank" href="{project.github}">Github</a>
    							<a class="card-link" target="_blank" href="{project.link}">View</a>
							</Card>
						</Collapsible>
					</div>
				{/each}
			{/if}
			<div class="spacer">

			</div>
		</div>
	</div>
	
</main>

<style>
	h2, h4 {
		text-align: center;
	}

	.collapsible-container {
		margin-bottom: 10px;
	}

	.no-bottom {
		margin-bottom: 0;
	}

	.spacer {
		height: 66vh;
	}
</style>