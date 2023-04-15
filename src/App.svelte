<script lang="ts">
  import {
    Header,
    Content,
    SkipToContent,
    HeaderUtilities,
    HeaderAction,
    DatePickerInput,
    DatePicker,
    Button
  } from 'carbon-components-svelte';
  import 'carbon-components-svelte/css/white.css';
  import MaplibreMap from './lib/Map.svelte';
  import EarthFilled from 'carbon-icons-svelte/lib/EarthFilled.svelte';
  import Satellite from 'carbon-icons-svelte/lib/Satellite.svelte';
  import moment from 'moment';

  let baseMapDate: string = (moment(new Date())).format('YYYY-MM-DD');
  let satellitePictureSwitch: boolean = true;
</script>

<main>
  <Header company="lexxamcode" platformName="Satellite map">
    <svelte:fragment slot="skip-to-content">
      <SkipToContent/>
    </svelte:fragment>
    <HeaderUtilities>
      <Button kind="secondary" iconDescription="Satellite layer" tooltipPosition="left" icon={Satellite} on:click={() => satellitePictureSwitch = !satellitePictureSwitch}/>
      <HeaderAction
        icon = {EarthFilled}
        closeIcon = {EarthFilled}>

        <DatePicker datePickerType="single" on:change dateFormat="Y-m-d" bind:value={baseMapDate}>
          <DatePickerInput labelText="Date"/>
        </DatePicker>
      </HeaderAction>     
        
    </HeaderUtilities>
  </Header>

  <Content>
    <MaplibreMap bind:baseMapDate={baseMapDate} bind:satellitePictureSwitch={satellitePictureSwitch}/>
  </Content>
</main>
