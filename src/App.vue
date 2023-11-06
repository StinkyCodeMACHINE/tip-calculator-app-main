<script setup>
import { ref } from "vue";
const inputs = ref({
  bill: {
    value: "",
    invalid: false,
  },
  tips: {
    selectedTip: null,
    tipOptions: [
      {
        tipAmount: "5",
      },
      {
        tipAmount: "10",
      },
      {
        tipAmount: "15",
      },
      {
        tipAmount: "25",
      },
      {
        tipAmount: "50",
      },
    ],
    customTip: {
      tipAmount: "",
    },
  },
  numOfPeople: {
    value: "",
    invalid: false,
  },
});
function isNumber(evt) {
  const keysAllowed = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9", "."];
  const keyPressed = evt.key;

  if (!keysAllowed.includes(keyPressed)) {
    evt.preventDefault();
  }
}
</script>

<template>
  <main>
    <h1 class="sr-only">Tip calculator app</h1>
    <img src="/assets/logo.svg" alt="word splitter" />
    <div class="calculator">
      <div class="calculator__inputs">
        <div class="calculator__inputs__bill">
          <div class="calculator__inputs__bill__label">Bill</div>
          <input
            type="text"
            name="bill"
            id="bill"
            :value="inputs.bill.value"
            @input="
              ($event) => {
                if ($event.target.value === `0`) {
                  inputs.bill.invalid = true;
                } else {
                  inputs.bill.invalid = false;
                }
                inputs.bill.value = $event.target.value;
              }
            "
            :class="{ invalid: inputs.bill.invalid }"
            placeholder="0"
            @keypress="isNumber($event)"
          />
          <div>Can't be zero</div>
        </div>
        <div class="calculator__inputs__tip-selection">
          <div class="calculator__inputs__tip-selection__label">
            Select Tip %
          </div>
          <div class="calculator__inputs__tip-selection__tip-options">
            <div
              class="calculator__inputs__tip-selection__tip-options__tip"
              :class="{ clicked: inputs.tips.selectedTip === index }"
              v-for="(tip, index) in inputs.tips.tipOptions"
              @click="inputs.tips.selectedTip = index"
              @mouseover="
                ($event) => {
                  $event.target.classList.add(`hovered-over`);
                }
              "
              @mouseleave="
                ($event) => {
                  $event.target.classList.remove(`hovered-over`);
                }
              "
            >
              {{ `${tip.tipAmount}%` }}
            </div>
            <input
              class="calculator__inputs__tip-selection__tip-options__tip"
              type="text"
              placeholder="Custom"
              v-model="inputs.tips.customTip.tipAmount"
              :style="{
                backgroundColor:
                  inputs.tips.selectedTip === 5
                    ? `transparent`
                    : `var(--very-light-grayish-cyan)`,
                outline:
                  inputs.tips.selectedTip === 5
                    ? `3px solid var(--strong-cyan)`
                    : `none`,
              }"
              @keypress="isNumber($event)"
              @click="inputs.tips.selectedTip = 5"
            />
          </div>
        </div>
        <div class="calculator__inputs__num-of-people">
          <div class="calculator__inputs__num-of-people__label">
            Number of People
          </div>
          <input
            type="text"
            name="num-of-people"
            id="num-of-people"
            :value="inputs.numOfPeople.value"
            @input="
              ($event) => {
                if ($event.target.value === `0`) {
                  inputs.numOfPeople.invalid = true;
                } else {
                  inputs.numOfPeople.invalid = false;
                }
                inputs.numOfPeople.value = $event.target.value;
              }
            "
            :class="{ invalid: inputs.numOfPeople.invalid }"
            placeholder="0"
            @keypress="isNumber($event)"
          />
          <div>Can't be zero</div>
        </div>
      </div>
      <div class="calculator__results">
        <div class="calculator__results__amount">
          <div class="calculator__results__amount__label">
            Tip Amount
            <div>/ person</div>
          </div>
          <div class="calculator__results__amount__value">
            {{
              inputs.bill.value &&
              inputs.numOfPeople.value &&
              inputs.tips.selectedTip !== null
                ? `$${
                    inputs.tips.selectedTip < 5
                      ? (
                          (Number(inputs.bill.value) *
                            Number(
                              inputs.tips.tipOptions[inputs.tips.selectedTip]
                                .tipAmount
                            ) *
                            0.01) /
                          Number(inputs.numOfPeople.value)
                        ).toFixed(2)
                      : inputs.tips.customTip.tipAmount
                      ? (
                          (Number(inputs.bill.value) *
                            Number(inputs.tips.customTip.tipAmount) *
                            0.01) /
                          Number(inputs.numOfPeople.value)
                        ).toFixed(2)
                      : `0.00`
                  }`
                : `$0.00`
            }}
          </div>
        </div>
        <div class="calculator__results__total">
          <div class="calculator__results__total__label">
            Total
            <div>/ person</div>
          </div>
          <div class="calculator__results__total__value">
            {{
              inputs.bill.value &&
              inputs.numOfPeople.value &&
              inputs.tips.selectedTip !== null
                ? `$${
                    inputs.tips.selectedTip < 5
                      ? (
                          (Number(inputs.bill.value) +
                            Number(inputs.bill.value) *
                              Number(
                                inputs.tips.tipOptions[inputs.tips.selectedTip]
                                  .tipAmount
                              ) *
                              0.01) /
                          Number(inputs.numOfPeople.value)
                        ).toFixed(2)
                      : inputs.tips.customTip.tipAmount
                      ? (
                          (Number(inputs.bill.value) +
                            Number(inputs.bill.value) *
                              Number(inputs.tips.customTip.tipAmount) *
                              0.01) /
                          Number(inputs.numOfPeople.value)
                        ).toFixed(2)
                      : `0.00`
                  }`
                : `$0.00`
            }}
          </div>
        </div>
        <button
          @click="
            () => {
              inputs.bill.value = ``;
              inputs.numOfPeople.value = ``;
              inputs.bill.invalid = false;
              inputs.numOfPeople.invalid = false;

              inputs.tips.customTip.tipAmount = ``;
              inputs.tips.selectedTip = null;
            }
          "
          class="calculator__results__reset-button"
          :disabled="!inputs.bill || !inputs.numOfPeople"
        >
          RESET
        </button>
      </div>
    </div>
  </main>
</template>

<style scoped></style>
