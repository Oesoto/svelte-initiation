<script>
  import Surname from "./Surname.svelte";
  import Box from "./Box.svelte";
  /*
	Using let any other component can use this component and pass
	name as a prop
  */
  export let name;
  let isChecked = false
  //Reactive declaration. Will automatically change when isChecked is changed
  $: opposite = !isChecked;

  let cities = [
	  {
		  id: 1,
		  name: "London"
	  },
	  {
		  id: 2,
		  name: "Berlin"
	  },
	  {
		  id: 3,
		  name: "Madrid"
	  }
	  
  ]

  //Async function
  async function getRandomStarWarsCharacter() {
    const randomNumber = Math.floor(Math.random() * 10) + 1;
    const apiResponse = await fetch(
      `https://swapi.co/api/people/${randomNumber}/`
    );

    return await apiResponse.json();
  }

  let starwarsPromise = getRandomStarWarsCharacter();
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    font-size: 3em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h1>Hello {name}!</h1>
  <!-- Calling Surname component with "surname" prop. Surname must export a "surname" variable -->
  <Surname surname={'apellido'} />
  <Box />
  <!-- Binding: name will change when text area value changes -->
  <textarea name="" id="" cols="30" rows="10" bind:value={name} />
  <label for="">
	<!-- Binding: Checkbox will alter "isChecked" value -->
    <input type="checkbox" bind:checked={isChecked} />
    Mark this checkbox
  </label>
  <p>Checkbox checked: {isChecked}</p>
  <!-- Conditional rendering -->
  {#if isChecked}
  	<p>Message when checkbox is checked</p>
  {/if}

  <p>Opposite value for checkbox: {opposite}</p>

  <!-- Loop and render -->
  <ul>
	{#each cities as city, index}
	<!-- Destructuring is allowed -->
	<!-- {#each cities as {name}, index} -->
		 <!-- <li>{index+1} - {name}</li> -->
		 <li>{index+1} - {city.name}</li>
	{/each}
  </ul>

	<h1>Async operation - StarWars Character</h1>
  <!-- Async operations -->
  {#await starwarsPromise}
	<!-- promise is pending -->
	<h1>Loading star wars character</h1>
  {:then character}
	<!-- promise was fulfilled -->
	<h1>{character.name}</h1>
  {/await}


</main>
