<script>
  import { Col, Row, Form, FormGroup, Input, Label, Button } from "sveltestrap";
  import { createEventDispatcher } from "svelte";

  let term = "";
  $: canSubmit = term.length > 0;

  const dispatch = createEventDispatcher();

  const onSearch = (e) => {
    e.preventDefault();
    canSubmit && dispatch("search", term);
    term = "";
  };
</script>

<Form on:submit={onSearch}>
  <Row>
    <Col md="8" xs="12">
      <FormGroup>
        <Label for="dadJokesSearch">Search</Label>
        <Input
          type="text"
          bind:value={term}
          id="dadJokesSearch"
          placeholder="Search for your favorite dad jokes..."
        />
      </FormGroup>
    </Col>
    <Col md="4" xs="12">
      <Button type="submit" disabled={!canSubmit} color="primary" class="search"
        >Search</Button
      >
    </Col>
  </Row>
</Form>

<style>
  :global(.search) {
    width: 100%;
    margin-top: 30px;
  }
  :global(button:disabled) {
    cursor: not-allowed;
  }
  @media screen and (max-width: 700px) {
    :global(.search) {
      margin-top: 0px;
      margin-bottom: 20px;
    }
  }
</style>
