<template>
  <div>
    <v-btn color="success">Device 1</v-btn>
    <div class="mapContainer">
      <GmapMap
        :center="{lat: 13.7478819, lng: 100.5676944}"
        :zoom="13"
        style="width: 100%; height: inherit"
      >
        <GmapMarker
          :position="{lat: 13.7478819, lng: 100.5676944}"
        />
      </GmapMap>
    </div>
</div>
</template>

<script>
export default {
  name: 'GoogleMap',
};
</script>

<style src="./Google.scss" lang="scss" scoped/>
