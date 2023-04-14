<script>
	const workList = [
		'parachoque delantero',
		'parachoque trasero',
		'tapabarro delantero izquierdo',
		'tababarro delantero derecho',
		'tapabarro trasero izquierdo',
		'tapabarro trasero derecho',
		'capot',
		'puerta delantera izquierda',
		'puerta delantera derecha',
		'puerta trasera izquierda',
		'puerta trasera derecha',
		'techo',
		'maleta',
	]

	let extraWorkList = [
		'cuadratura de chasis',
		'corte de chasis',
		'cambio de maleta',
		'cambio de focos',
		'cambio de parachoque',
		'cambio de refuerzo de parachoque',
		'pintura al horno',
		'pulido',
	]

	const buildWorkId = n => `work-${n}`

	const slugify = str => str.toLowerCase().replaceAll(' ', '-')

	const capitalize = str => str[0].toUpperCase() + str.slice(1).toLowerCase()

	const handleExtraWork = event => {
		if (event.code == 'Enter') {
			extraWorkList = [...extraWorkList, event.srcElement.value.toLowerCase()]
			event.srcElement.value = '' // reset search box
		}
	}
</script>

<header>
	<h1>Nueva cotización</h1>
</header>

<main>
	<form>
		<label for="name">Nombre:</label>
		<input type="text" name="name" id="name" required />

		<label for="brand">Marca:</label>
		<input type="text" name="brand" id="brand" required />

		<label for="model">Modelo:</label>
		<input type="text" name="model" id="model" required />

		<label for="date">Fecha:</label>
		<input type="date" name="date" id="date" required />

		<label for="price">Precio:</label>
		<input type="number" name="price" id="price" min="1000" step="1000" list="defaultPrices" required />

		<!-- TODO this is unnecessary, but I thought it may be built dynamically according to ticked work checkboxes -->
		<datalist id="defaultPrices">
			<option value="100000"></option>
			<option value="500000"></option>
			<option value="1000000"></option>
		</datalist>

		<!-- TODO these should come from a DB query -->
		<fieldset>
			<legend><strong>Partes a tratar</strong></legend>
				{#each workList as work, index (work)}
					<label for={buildWorkId(index)}>
						<input type="checkbox" id={buildWorkId(index)} name={slugify(work)} />
						{capitalize(work)}	
					</label>
				{/each}
		</fieldset>

		<!-- TODO the input box should work as a search box, querying the backend -->
		<fieldset>
			<legend><strong>Otros trabajos a realizar</strong></legend>

			<label for="search">Search</label>
			<input type="search" id="search" name="search" placeholder="Cuadratura de chasis" on:keydown={handleExtraWork} />

			<ul>
				{#each extraWorkList as work (work)}
					<li>{capitalize(work)}</li>
				{/each}
			</ul>
		</fieldset>

		<input type="submit" value="Guardar" onclick="event.preventDefault()" />
	</form>
</main>
