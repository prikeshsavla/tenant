<script setup>
const generate = (event) => {
  console.log(Object.fromEntries(new FormData(event.target).entries()));
};
const print = () => {
  window.print();
};
</script>

<template>
  <hgroup>
    <h1>Tenant</h1>
    <h2>Generate Rent Receipt</h2>
  </hgroup>
  <details class="hidden-print" open>
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
              placeholder="Your Name"
              required
            />
          </label>
          <label>
            Rented Address*
            <input type="text" name="address" placeholder="Address" required />
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
              placeholder="PAN No"
              required
            />
            <small>
              PAN of landlord is required, if rent exceeds Rs.8333/- per month.
              In case landlord does not have PAN, declaration of the same is
              required.
            </small>
          </label>
        </div>
      </div>

      <div class="grid">
        <div>
          <label for="date"
            >Start Date
            <input type="date" name="startDate" />
          </label>
        </div>
        <div>
          <label for="date"
            >End Date
            <input type="date" name="endDate" />
          </label>
        </div>
      </div>
      <div>
        <label for="fruit">How would you like your receipts to be? </label>
        <select name="format">
          <option value="M" selected>Monthly</option>
          <option value="Q">Quarterly</option>
          <option value="HY">Half Yearly</option>
          <option value="Y">Yearly</option>
        </select>
      </div>
      <div>
        <button>Generate</button>
      </div>
    </form>
  </details>

  <details>
    <summary class="hidden-print">Rent Receipts</summary>
    <div class="overflow">
      <article>
        <header>
          <hgroup class="headings">
            <h4>Rent Receipt</h4>
            <h5>March 2021</h5>
          </hgroup>
        </header>
        <p>
          Received sum of <strong>25000</strong> from Tenant (Tenant PAN)
          towards the rent of property located at Address for the period
          <strong>March 2021</strong>
        </p>
        <footer>
          <p>
            Signature <br />
            LANDLORD NAME <br />
            LANDLORD PAN
          </p>
        </footer>
      </article>
    </div>
    <button class="secondary hidden-print" @click="print">Print All</button>
  </details>
</template>

<style scoped>
h1 {
  margin-bottom: calc(var(--spacing) * 0);
}
article hgroup {
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
article footer p {
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
  }
}
@media screen {
  .overflow {
    height: 50vh;
    overflow-y: scroll;
    padding: calc(var(--block-spacing-horizontal) * 0.5);
  }
}
</style>
