<script lang="ts">
  import { formatDate } from '$lib/utils/util';
  import { Badge } from 'sveltestrap';
  import type { Procedure } from "fhir/r4";
  import type { ResourceTemplateParams } from '$lib/utils/types';
  
  export let content: ResourceTemplateParams<Procedure>; // Define a prop to pass the data to the component
  let resource: Procedure = content.resource;
</script>

{#if resource.code}
  {#if resource.code.coding}
    <Badge color="primary">{resource.code.coding[0].system} : {resource.code.coding[0].code}</Badge>
    <br />
    {#if resource.code.coding[0].display}
      <strong>{resource.code.coding[0].display}</strong><br>
    {:else if resource.code.text}
      <strong>{resource.code.text}</strong><br>
    {/if}
  {:else if resource.code.text}
    <strong>{resource.code.text}</strong><br>
  {/if}
{/if}
{#if resource.performedDateTime}
  Performed: {formatDate(resource.performedDateTime)}
{:else if resource.performedPeriod}
  Performed: {
    resource.performedPeriod.start
    ? formatDate(resource.performedPeriod.start)
    : '??'
  } - {
    resource.performedPeriod.end
    ? formatDate(resource.performedPeriod.end)
    : '??'
  }
{:else if resource.performedString}
  Performed: {resource.performedString}
{:else if resource.performedAge}
  Performed age: {resource.performedAge.value}{resource.performedAge.code}
{:else if resource.performedRange}
  Performed age: {
    resource.performedRange.low
    ? resource.performedRange.low
    : '??'
  } - {
    resource.performedRange.high
    ? resource.performedRange.high
    : '??'
  }
{/if}
