<script>
  import { onMount } from "svelte";
  import { getRandomJoke, getSearchJokes } from "./requests.js";
  import { Button, Col, Container, Row } from "sveltestrap";
  import JokeForm from "./Form.svelte";
  import Joke from "./Joke.svelte";
  import Transition from "./Transition.svelte";

  let randomJoke;
  let jokes = [];
  let mode = "loading";

  onMount(async () => {
    await onRandomJoke();
  });

  async function onRandomJoke() {
    try {
      mode = "loading";
      randomJoke = await getRandomJoke();
      mode = "random";
    } catch (error) {
      alert("Don't panic...but there was an error.");
    }
  }

  const onSearch = async (e) => {
    try {
      mode = "loading";
      jokes = await getSearchJokes(e.detail);
      mode = "search";
    } catch (error) {
      alert("Don't panic...but there was an error.");
    }
  };
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css"
  />
</svelte:head>

<Container>
  <Row>
    <Col>
      <h1>Dad Jokes!!! :)</h1>
    </Col>
  </Row>
  <JokeForm on:search={onSearch} />
  <Row>
    <Col>
      <Button on:click={onRandomJoke} class="random-joke" color="danger">
        Random Joke
      </Button>
    </Col>
  </Row>
  {#if mode === "random"}
    <Transition>
      <Row>
        <Col>
          <Joke joke={randomJoke} />
        </Col>
      </Row>
    </Transition>
  {/if}

  {#if mode === "search" && jokes.length > 0}
    <Transition>
      {#each jokes as jokeObj (jokeObj.id)}
        <Row>
          <Col>
            <Joke joke={jokeObj.joke} />
          </Col>
        </Row>
      {/each}
    </Transition>
  {/if}
  {#if mode === "search" && jokes.length == 0}
    <Transition>
      <Row>
        <Col>
          <Joke joke="Jokes on you!!! :) Please try another search." />
        </Col>
      </Row>
    </Transition>
  {/if}
</Container>

<style>
  :global(.random-joke) {
    width: 100%;
    margin-top: 20px;
  }
</style>
