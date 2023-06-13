<script setup lang="ts"></script>

<template>
  <div id="page-container">
    <section class="main-section">
      <div class="main-row">
        <div class="left-col">
          <div class="card front">
            <div class="cirlces">
              <div class="circle-big"></div>
              <div class="circle-small"></div>
            </div>
            <div class="card-number">0000 0000 0000 0000</div>
            <div class="card-info">
              <div class="card-name">JANE APPLESEED</div>
              <div class="card-date">
                <span class="card-month">00</span>
                <span>/</span>
                <span class="card-year">00</span>
              </div>
            </div>
          </div>
          <div class="card back">
            <div class="card-code">000</div>
          </div>
        </div>
        <div class="right-col">
          <form>
            <div class="form-row">
              <label for="name">CARDHOLDER NAME</label>
              <input
                type="text"
                name="name"
                id="name"
                placeholder="e.g. Jane Appleseed"
              />
            </div>
            <div class="form-row">
              <label for="number">CARD NUMBER</label>
              <input
                type="number"
                name="number"
                id="number"
                placeholder="e.g. 1234 5678 9123 0000"
              />
            </div>
            <div class="form-row split-two">
              <div class="left">
                <label for="month">Exp. Date (MM/YY)</label>
                <div class="inputs-container">
                  <input type="number" name="month" id="month" />
                  <input type="number" name="year" id="year" />
                </div>
              </div>
              <div class="right">
                <label for="cvc">CVC</label>
                <input type="number" name="cvc" id="cvc" />
              </div>
            </div>
            <div class="form-actions">
              <button type="submit">Confirm</button>
            </div>
          </form>
        </div>
      </div>
    </section>
  </div>
</template>
