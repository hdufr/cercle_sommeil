<script>
  import { Styles } from "sveltestrap";
  import {
    Button,
    Modal,
    ModalBody,
    ModalFooter,
    ModalHeader,
  } from "sveltestrap";
  import { Form, FormGroup, Input, Label } from "sveltestrap";

  export let open;
  export let timing ; //en nb de seconde
  export let session ; //en nb de minutes

  let formTiming = 5;
  let formSession = 20;

  const toggle = () => {
    open = !open;
    formTiming = timing;
  };

  const validate = () => {
    timing = formTiming;
    session = formSession;
    open = false
  };
</script>

<Styles />

<Modal isOpen={open} {toggle}>
  <ModalHeader {toggle}>Réglages</ModalHeader>
  <ModalBody>
    <Form>
      <FormGroup>
        <Label for="formTiming"
          >Cycle (valeur actuelle en secondes: {formTiming})</Label
        >
        <Input
          class="border-0"
          type="range"
          name="range"
          id="formTiming"
          min={4}
          max={7}
          step={0.5}
          placeholder="Range placeholder"
          bind:value={formTiming}
        />
      </FormGroup>
      <FormGroup>
        <Label for="formSession"
          >Session (valeur actuelle en minutes: {formSession})</Label
        >
        <Input
          class="border-0"
          type="range"
          name="range"
          id="formSession"
          min={5}
          max={60}
          step={5}
          placeholder="Range placeholder"
          bind:value={formSession}
        />
      </FormGroup>
    </Form>
  </ModalBody>
  <ModalFooter>
    <Button color="success" on:click={validate}>Valider</Button>
    <Button color="secondary" on:click={toggle}>Annuler</Button>
  </ModalFooter>
</Modal>
