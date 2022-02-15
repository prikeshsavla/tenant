<script setup>
import { reactive, computed } from "vue";
import * as moment from "moment";

let state = reactive({ rentInfo: {} });

const print = () => {
  window.print();
};
const generate = (event) => {
  state.rentInfo = Object.fromEntries(new FormData(event.target).entries());
  document.getElementById("tenant-details").removeAttribute("open");
};

// a computed ref
const rentReceipts = computed(() => {
  const { startDateString, endDateString } = state.rentInfo;
  var startDate = moment(startDateString);
  var endDate = moment(endDateString);

  var result = [];

  if (endDate.isBefore(startDate)) {
    throw "End date must be greated than start date.";
  }

  while (startDate.isBefore(endDate)) {
    result.push({ month: startDate.format("MMMM YYYY"), ...state.rentInfo });
    startDate.add(1, "month");
  }
  return result;
});
</script>

<template>
  <section>
    <div class="hidden-print">
      <h1>Tenant</h1>
      <p>Generate Rent Receipts</p>

      <details id="tenant-details" open>
        <summary>Tentant Details</summary>
        <form @submit.prevent="generate">
          <div class="grid">
            <label>
              Your Name*
              <input
                type="text"
                name="tenantName"
                placeholder="Your Name"
                required
              />
            </label>
            <label>
              Monthly Rent*
              <input
                type="number"
                name="rent"
                placeholder="Monthly Rent"
                required
              />
            </label>
          </div>
          <div class="grid">
            <div>
              <label>
                Your PAN (optional)
                <input
                  type="text"
                  name="tenantPan"
                  placeholder="Your PAN No"
                  required
                />
              </label>
              <label>
                Rented Address*
                <input
                  type="text"
                  name="address"
                  placeholder="Address"
                  required
                />
              </label>
            </div>
            <div>
              <label>
                Name of Landlord*
                <input
                  type="text"
                  name="landlordName"
                  placeholder="Name of Landlord"
                  required
                />
              </label>
              <label>
                Landlord PAN No.
                <input
                  type="text"
                  name="landlordPan"
                  placeholder="Landlord PAN No"
                  required
                />
                <small>
                  PAN of landlord is required, if rent exceeds Rs.8333/- per
                  month. In case landlord does not have PAN, declaration of the
                  same is required.
                </small>
              </label>
            </div>
          </div>

          <div class="grid">
            <div>
              <label for="date"
                >Start Date
                <input type="date" name="startDateString" />
              </label>
            </div>
            <div>
              <label for="date"
                >End Date
                <input type="date" name="endDateString" />
              </label>
            </div>
          </div>
          <fieldset>
            <legend>How would you like your receipts to be?</legend>

            <label for="small">
              <input
                type="radio"
                id="small"
                name="size"
                value="small"
                checked
              />
              Monthly
            </label>
            <label for="medium">
              <input type="radio" id="medium" name="size" value="medium" />
              Quarterly
            </label>
          </fieldset>
          <div>
            <button>Generate</button>
          </div>
        </form>
      </details>
    </div>
    <details v-if="rentReceipts.length" id="rent-receipts" open>
      <summary class="hidden-print">Rent Receipts</summary>
      <div class="overflow">
        <article v-for="receipt in rentReceipts" :key="receipt.month">
          <header>
            <h3>Rent Receipt</h3>
            {{ receipt.month }}
          </header>
          <p>
            Received sum of <strong>{{ receipt.rent }}</strong> from
            <strong>{{ receipt.tenantName }} ({{ receipt.tenantPan }})</strong>
            towards the rent of property located at
            <i>{{ receipt.address }}</i> for the period
            <strong>{{ receipt.month }}</strong>
          </p>
          <p>
            Signature <br />
            {{ receipt.landlordName }} ({{ receipt.landlordPan }})
          </p>
        </article>
      </div>
      <button class="secondary hidden-print" @click="print">Print All</button>
    </details>
  </section>
</template>

<style scoped>
h1 {
  margin-bottom: calc(var(--spacing) * 0);
}
article header h3,
article header p {
  margin-bottom: 0;
}
form {
  margin-top: calc(var(--spacing) * 1);
}
article {
  --block-spacing-vertical: calc(var(--spacing) * 2);
  border: 1px solid var(--muted-border-color);
  box-shadow: none;
}
article p:last-child {
  padding-top: calc(var(--block-spacing-vertical) * 2);
  margin-bottom: 0;
  text-align: right;
}
@media print {
  .hidden-print {
    display: none;
  }
  article {
    border: 1px solid var(--muted-border-color);
    box-shadow: none;
    page-break-inside: avoid;
  }
}
@media screen {
  .overflow {
    height: 50vh;
    overflow-y: scroll;
    padding: calc(var(--block-spacing-horizontal) * 0.5);
  }
}
/* Style the tab */
.tab {
  overflow: hidden;
}

/* Style the tab content */
.tabcontent {
  display: none;
}
</style>
