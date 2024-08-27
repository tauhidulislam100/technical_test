<script setup lang="ts">
import { api } from '@/api';
import { ref } from 'vue';
import { useModal } from '@/composables/useModal';
import { useToast } from '@/composables/useToast';
const modal = useModal<boolean>();
const toast = useToast();

const formData = ref({
  applicantName: '',
  applicantEmail: '',
  applicantMobilePhoneNumber: '',
  applicantAddress: '',
  annualIncomeBeforeTax: 0,
  incomingAddress: '',
  incomingDeposit: 0,
  incomingPrice: 0,
  incomingStampDuty: 0,
  loanAmount: 0,
  loanDuration: 0,
  monthlyExpenses: 0,
  outgoingAddress: '',
  outgoingMortgage: 0,
  outgoingValuation: 0,
  savingsContribution: 0,
});

const submitApplication = async () => {
  const response = await api.applications.post(formData.value);
  if (response.success) toast.success('Application Saved Successfully.');
  else {
    toast.error('Error occurred while saving application');
    formData.value.applicantName = '';
    formData.value.applicantEmail = '';
    formData.value.applicantMobilePhoneNumber = '';
    formData.value.applicantAddress = '';
    formData.value.annualIncomeBeforeTax = 0;
    formData.value.incomingAddress = '';
    formData.value.incomingDeposit = 0;
    formData.value.incomingPrice = 0;
    formData.value.incomingStampDuty = 0;
    formData.value.loanAmount = 0;
    formData.value.loanDuration = 0;
    formData.value.monthlyExpenses = 0;
    formData.value.outgoingAddress = '';
    formData.value.outgoingMortgage = 0;
    formData.value.outgoingValuation = 0;
    formData.value.savingsContribution = 0;
  }
  modal.confirm(false);
};
</script>

<template>
  <div class="action-section">
    <BCard align-title="center" align-footer="center" align-content="center">
      <template #title>Submit loan application</template>
      <BSvgIcon name="dashboard-loan" />
      <template #footer>
        <BButton variant="primary" label="Submit application" icon-pos="right" icon="pi pi-chevron-right"
          @click="modal.showModal()" />
      </template>
    </BCard>

    <BModal :visible="modal.isVisible.value" :confirm="modal.confirm">
      <template #header>Submit loan application</template>

      <form @submit.prevent="submitApplication()">
        <!-- Need to change with v-for after change state with object -->
        <label for="applicant_name">Name</label>
        <BTextInput id="applicant_name" v-model="formData.applicantName" type="text" required />

        <label for="applicant_email">Email</label>
        <BTextInput id="applicant_email" v-model="formData.applicantEmail" type="email" required />

        <label for="applicant_mobile_phone_number">Mobile Phone Number</label>
        <BTextInput id="applicant_mobile_phone_number" v-model="formData.applicantMobilePhoneNumber" type="tel"
          required />

        <label for="applicant_address">Applicant Address</label>
        <BTextInput id="applicant_address" v-model="formData.applicantAddress" required />
        <label for="annual_income_before_tax">Annual Income Before Tax</label>
        <BNumberInput id="annual_income_before_tax" v-model="formData.annualIncomeBeforeTax" required />
        <label for="incoming_address">Incoming Address</label>
        <BTextInput id="incoming_address" v-model="formData.incomingAddress" required />
        <label for="incoming_deposit">Incoming deposit</label>
        <BNumberInput id="incoming_deposit" v-model="formData.incomingDeposit" required />

        <label for="incoming_price">Incoming Price</label>
        <BNumberInput id="incoming_price" v-model="formData.incomingPrice" required />
        <label for="incoming_stamp_duty">Incoming Stamp Duty</label>
        <BNumberInput id="incoming_stamp_duty" v-model="formData.incomingStampDuty" required />
        <label for="loan_amount">Loan Amount</label>
        <BNumberInput id="loan_amount" v-model="formData.loanAmount" required />
        <label for="loan_duration">Loan Duration</label>
        <BNumberInput id="loan_duration" v-model="formData.loanDuration" required />
        <label for="monthly_expenses">Monthly Expenses</label>
        <BNumberInput id="monthly_expenses" v-model="formData.monthlyExpenses" required />
        <label for="outgoing_address">Outgoing Address</label>
        <BTextInput id="outgoing_address" v-model="formData.outgoingAddress" required />
        <label for="outgoing_mortgage">Outgoing Mortgage</label>
        <BNumberInput id="outgoing_mortgage" v-model="formData.outgoingMortgage" required />
        <label for="outgoing_valuation">Outgoing Valuation</label>
        <BNumberInput id="outgoing_valuation" v-model="formData.outgoingValuation" required />
        <label for="savings_contribution">Savings Contribution</label>
        <BNumberInput id="savings_contribution" v-model="formData.savingsContribution" required />
      </form>

      <template #footer>
        <BButton type="submit" variant="primary" label="Submit" @click="submitApplication"></BButton>
        <BButton label="Cancel" @click="modal.confirm(false)"></BButton>
      </template>
    </BModal>
  </div>
</template>

<style lang="scss" scoped>
.action-section {
  display: flex;
  flex-flow: row wrap;
  gap: 1rem;
  align-items: stretch;
  container-type: inline-size;

  >* {
    flex: 1 1 100%;
  }

  @container (min-width: 900px) {
    >* {
      flex: 1 1 calc((100% - 2rem) / 3);
    }
  }
}

.b-card {
  height: 100%;
}

.b-icon {
  width: 5rem;
  height: 5rem;
}
</style>
