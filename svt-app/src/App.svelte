<script>
	let name = 'Armando'
	let v1 = 0
	let v2 = 0
	let isCheck = false
	let text 
	const showAlert = () => alert('Hello')

	let counter = 0
	// Reactive variables
	$:resultTotal = counter * 100
	$: if(resultTotal > 500){
		text = 'Expensive'
	} else {
		text = 'Afordable'
	}

	let login = false
	const changeLogin = () => login = !login

	let names = ['Armand', 'Lulu', 'Carl']
	let people = [
					{"name":"Orlando Montgomery","phone":"(956) 528-4536","country":"Brazil"},
					{"name":"Lionel Thomas","phone":"(771) 164-8768","country":"Spain"},
					{"name":"Jasper Branch","phone":"(189) 337-1035","country":"Pakistan"},
					{"name":"Nasim Carlson","phone":"(587) 554-0602","country":"Netherlands"},
					{"name":"Armando Hicks","phone":"(446) 328-8617","country":"Pakistan"}
				]
	// people = []
	// ---------------------------------------
	function sleep(s){
		return new Promise( res => setTimeout(res, s))
	}

	let promesa = ajax()
	let users = []
	const url = 'https://jsonplaceholder.typicode.com/users'
	
	async function ajax(){
		console.log(url)
		
		const resp = await fetch(url)
		users = await resp.json()

		await sleep(3000)

		if (resp.ok) {
			return users
		} else {
			throw new Error('Error al conectar con la api')
		}
		
	}

</script>

<main>
	<p>{name}</p>
	<input type="text" 
		bind:value={name} 
		placeholder="Pleace write your name"
	>
	<hr>

	<strong>Suma</strong>
	<br>
	Valor 1 <input type="range" bind:value="{v1}">
	<br>
	Valor 2 <input type="range" bind:value="{v2}">
	<p>{v1} + {v2} = {v1 + v2} </p>
	<hr>

	Do you accept the terms and conditions? 
	<input type="checkbox" bind:checked={isCheck}>
	<br>
	<button disabled={!isCheck} on:click={showAlert}>Guardar</button>
	<hr>

	<button on:click={() => counter += 1}>Contador</button>
	<p>Contador : {counter}, Resultado x 100 = {resultTotal}</p>
	<p>Status: {text} </p>
	<hr>

	<p><strong>IF</strong> BLOCK</p>
	{#if login}
		<h4>Estas logeado</h4>
	{:else}
		<h4>Inicia sesión</h4>
	{/if}
	<button on:click="{changeLogin}">Login</button>
	<hr>

	<p><strong>EACH</strong> BLOCK</p>
	<ul>

	</ul>
	{#each names as name, index}
		<li>{index+1} {name}</li>
	{/each}

	<table>
		<thead>
			<tr>
				<th>Name</th>
				<th>Phone</th>
				<th>Country</th>
			</tr>
		</thead>
		<tbody>	
			{#each people as person}
				<tr>
					<td>{person.name}</td>
					<td>{person.phone} </td>
					<td>{person.country}</td>
				</tr>
			{:else}
				<tr>
					<td colspan="3">No existen registros</td>
				</tr>
			{/each}
		</tbody>
	</table>
	<hr>
	<p><strong>AWAIT</strong> BLOCK</p>
	{#await promesa}
		<p style="color:blue;">Cargando api</p>
	{:then users}
		<ul>
		{#each users as u}
			<li>{u.name}</li>
		{/each}
		</ul>
	{:catch error}
		<p style="color:red;">No se pudo conectar a la API {error}</p>
	{/await}


</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	h2 {
		color : blue;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>