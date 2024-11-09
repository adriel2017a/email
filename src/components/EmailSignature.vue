<template>
  <div style="display: flex; flex-direction: column; gap: 20px;">
    <h2>Email Signature Generator</h2>

    <!-- Input Form Section -->
    <div>
      <div class="input-group">
        <label for="name">Name:</label>
        <input type="text" v-model="name" placeholder="Enter your name" />
      </div>
      <div class="input-group">
        <label for="position">Position:</label>
        <input type="text" v-model="position" placeholder="Enter your position" />
      </div>
      <div class="input-group">
        <label for="email">Email:</label>
        <input type="email" v-model="email" placeholder="Enter your email" />
      </div>
      <div class="input-group">
        <label for="location">Location:</label>
        <input type="text" v-model="location" placeholder="Austin | Seoul | Paris" />
      </div>
      <div class="input-group">
        <label for="imageURL">Profile Image URL:</label>
        <input type="text" v-model="imageURL" placeholder="Paste image URL here" />
      </div>
    </div>

    <!-- Signature Preview Section -->
    <table class="signature-table" style="border-collapse: collapse; width: 100%; max-width: 500px;">
      <tr style="vertical-align: top;">
        <!-- Left Column: Profile Image and Logo -->
        <td style="width: 120px; text-align: center; vertical-align: bottom;">
          <img :src="imageURL || 'https://i.imgur.com/ZcLLrkY.jpg'" alt="Profile Image" class="profile-image" style="width: 100px; height: 100px; display: block;">
          <img src="https://i.imgur.com/PdAtavv.png" alt="Adriel Logo" class="logo" style="width: 100px; display: block; margin-top: 18px;">
        </td>

        <!-- Right Column: Name, Position, and Contact Information -->
        <td style="padding-left: 15px;">
          <table style="width: 100%; line-height: 1.6;">
            <tr>
              <td style="font-size: 18px; font-weight: bold;">
                {{ name || 'Luke Kim' }}
              </td>
            </tr>
            <tr>
              <td style="font-size: 14px; color: #555;">
                {{ position || 'Customer Success Manager' }}
              </td>
            </tr>
            <tr>
              <td style="padding-top: 10px; border-top: 1px solid #4A6EAD;">
                📧 <a :href="`mailto:${email}`" style="text-decoration: none; color: #333;">{{ email || 'luke.kim@adriel.com' }}</a>
              </td>
            </tr>
            <tr>
              <td>
                🔗 <a href="http://www.adriel.com" style="text-decoration: none; color: #333; font-weight: normal;">www.adriel.com</a>
              </td>
            </tr>
            <tr>
              <td>
                📍 {{ location || 'Austin | Seoul | Paris' }}
              </td>
            </tr>
          </table>
        </td>
      </tr>
      <!-- SNS Icons Row aligned to match Gmail layout -->
      <tr>
        <td colspan="2" style="text-align: center; padding-top: 15px;">
          <div class="social-icons" style="display: flex; justify-content: center; margin-top: 20px;">
            <a href="https://www.facebook.com/adrielmarketing" target="_blank">
              <img src="https://img.icons8.com/color/48/000000/facebook-circled--v1.png" alt="Facebook">
            </a>
            <a href="https://x.com/AdrielMarketing" target="_blank">
              <img src="https://img.icons8.com/ios-filled/50/000000/xbox-x.png" alt="X (Twitter)" style="width: 24px; height: 24px;">
            </a>
            <a href="https://www.linkedin.com/company/adrielmarketing/" target="_blank">
              <img src="https://img.icons8.com/color/48/000000/linkedin-circled--v1.png" alt="LinkedIn">
            </a>
            <a href="https://www.instagram.com/adriel.ads/" target="_blank">
              <img src="https://img.icons8.com/color/48/000000/instagram-new.png" alt="Instagram">
            </a>
          </div>
        </td>
      </tr>
    </table>

    <!-- Download Button -->
    <button @click="downloadSignature" style="padding: 10px 20px; font-size: 16px; margin-top: 20px;">Download HTML File</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      position: "",
      email: "",
      location: "",
      imageURL: ""
    };
  },
  methods: {
    downloadSignature() {
      const previewContent = document.querySelector(".signature-table").outerHTML;
      const htmlContent = `
      <!DOCTYPE html>
      <html lang="en">
      <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Email Signature</title>
        <style>
          body { font-family: Arial, sans-serif; line-height: 1.4; }
          .signature-table { width: 100%; max-width: 500px; border-collapse: collapse; }
          .profile-image { width: 100px; height: 100px; border-radius: 50%; display: block; }
          .social-icons { display: flex; justify-content: center; margin-top: 20px; } /* Centered SNS icons */
          .social-icons img { width: 24px; height: 24px; margin-right: 5px; }
          .logo { width: 100px; display: block; margin-top: 18px; }
          .divider { border: 0; border-top: 1px solid #4A6EAD; margin: 15px 0; }
          a[href*="www.adriel.com"] { text-decoration: none; color: #333 !important; font-weight: normal; } /* Force black color and remove underline */
        </style>
      </head>
      <body>
        ${previewContent}
      </body>
      </html>
      `;
      const blob = new Blob([htmlContent], { type: "text/html" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "email_signature.html";
      a.click();
      URL.revokeObjectURL(url);
    }
  }
};
</script>

<style scoped>
.signature-table { width: 100%; max-width: 500px; border-collapse: collapse; }
.profile-image { width: 100px; height: 100px; border-radius: 50%; display: block; }
.social-icons { display: flex; justify-content: center; margin-top: 20px; } /* Centered SNS icons for preview */
.social-icons img { width: 24px; height: 24px; margin-right: 5px; }
.logo { width: 100px; display: block; margin-top: 18px; }
.divider { border: 0; border-top: 1px solid #4A6EAD; margin: 15px 0; }
a[href*="www.adriel.com"] { text-decoration: none; color: #333 !important; font-weight: normal; } /* Force black color and remove underline */
</style>
