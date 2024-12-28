<header>

<<!DOCTYPE html>
<html lang="cs">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mini Bar Management</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            color: #333;
        }

        .container {
            width: 100%;
            max-width: 480px;
            margin: 20px auto;
            background: #fff;
            border-radius: 16px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            padding: 20px;
            box-sizing: border-box;
        }

        h1 {
            text-align: center;
            font-size: 28px;
            margin-bottom: 20px;
            text-transform: uppercase;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            font-size: 16px;
            font-weight: bold;
            margin-bottom: 8px;
        }

        .form-group input {
            width: 100%;
            font-size: 16px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 8px;
            box-sizing: border-box;
            margin-bottom: 10px;
        }

        button {
            display: block;
            width: 100%;
            max-width: 320px;
            margin: 20px auto;
            padding: 15px;
            background: linear-gradient(135deg, #3498db, #2ecc71);
            color: white;
            font-size: 18px;
            font-weight: bold;
            text-transform: uppercase;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
            transition: background 0.3s;
        }

        button:hover {
            background: linear-gradient(135deg, #2980b9, #27ae60);
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Inventář Mini Baru</h1>
        <div id="items"></div>
        <button onclick="generateInvoice()">Generovat fakturu</button>
    </div>

    <script>
        const API_URL = 'http://localhost:3000';

        // Načíst inventář
        async function loadInventory() {
            const res = await fetch(`${API_URL}/items`);
            const items = await res.json();
            const container = document.getElementById('items');
            container.innerHTML = '';

            items.forEach(item => {
                const formGroup = document.createElement('div');
                formGroup.classList.add('form-group');

                const label = document.createElement('label');
                label.textContent = `${item.name} (Skladem: ${item.quantity}, Cena: ${item.price} Kč)`;

                const input = document.createElement('input');
                input.type = 'number';
                input.min = '0';
                input.placeholder = 'Zadejte počet';
                input.id = `item-${item.id}`;

                formGroup.appendChild(label);
                formGroup.appendChild(input);
                container.appendChild(formGroup);
            });
        }

        // Odečíst položky a vytvořit fakturu
        async function generateInvoice() {
            const inputs = document.querySelectorAll('#items input');
            let invoiceDetails = '';
            let total = 0;

            for (const input of inputs) {
                const id = input.id.split('-')[1];
                const quantity = parseInt(input.value, 10);

                if (quantity > 0) {
                    const res = await fetch(`${API_URL}/items`);
                    const items = await res.json();
                    const item = items.find(i => i.id == id);

                    if (item) {
                        invoiceDetails += `${quantity}x ${item.name}: ${item.price * quantity} Kč<br>`;
                        total += item.price * quantity;

                        await fetch(`${API_URL}/deduct`, {
                            method: 'POST',
                            headers: { 'Content-Type': 'application/json' },
                            body: JSON.stringify({ id, quantity })
                        });
                    }
                }
            }

            alert(`Faktura:\n${invoiceDetails}\nCelkem: ${total} Kč`);
            loadInventory();
        }

        window.onload = loadInventory;
    </script>
</body>
</html>!--
  <<< Author notes: Course header >>>
  Read <https://skills.github.com/quickstart> for more information about how to build courses using this template.
  Include a 1280×640 image, course name in sentence case, and a concise description in emphasis.
  In your repository settings: enable template repository, add your 1280×640 social image, auto delete head branches.
  Next to "About", add description & tags; disable releases, packages, & environments.
  Add your open source license, GitHub uses the MIT license.
-->

# Code with GitHub Copilot

_GitHub Copilot can help you code by offering autocomplete-style suggestions right in VS Code and Codespaces._

</header>

<!--
  <<< Author notes: Course start >>>
  Include start button, a note about Actions minutes,
  and tell the learner why they should take the course.
-->

## Welcome

GitHub Copilot can help you code by offering autocomplete-style suggestions. You can learn how GitHub Copilot works, and what to consider while using GitHub Copilot. GitHub Copilot analyzes the context in the file you are editing, as well as related files, and offers suggestions from within your text editor. GitHub Copilot is powered by OpenAI Codex, a new AI system created by OpenAI.

- **Who this is for**: Developers, DevOps Engineers, Software development managers, Testers.
- **What you'll learn**: How to install Copilot into a Codespace, accept suggestions from code, accept suggestions from comments.
- **What you'll build**: Javascript files that will have code generated by Copilot AI for code and comment suggestions.
- **Prerequisites**: 
  - [GitHub account](https://github.com/login) - With available Codespaces minutes.
  - [GitHub Copilot](https://github.com/github-copilot/signup) - For learning, the **Copilot Free** option with usage limits should be sufficient.
- **Timing**: This course can be completed in under an hour.

### How to start this course

<!-- For start course, run in JavaScript:
'https://github.com/new?' + new URLSearchParams({
  template_owner: 'skills',
  template_name: 'copilot-codespaces-vscode',
  owner: '@me',
  name: 'skills-copilot-codespaces-vscode',
  description: 'My clone repository',
  visibility: 'public',
}).toString()
-->

[![start-course](https://user-images.githubusercontent.com/1221423/235727646-4a590299-ffe5-480d-8cd5-8194ea184546.svg)](https://github.com/new?template_owner=skills&template_name=copilot-codespaces-vscode&owner=%40me&name=skills-copilot-codespaces-vscode&description=My+clone+repository&visibility=public)

1. Right-click **Start course** and open the link in a new tab.
2. In the new tab, most of the prompts will automatically fill in for you.
   - For owner, choose your personal account or an organization to host the repository.
   - We recommend creating a public repository, as private repositories will [use Actions minutes](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions).
   - Scroll down and click the **Create repository** button at the bottom of the form.
3. After your new repository is created, wait about 20 seconds, then refresh the page. Follow the step-by-step instructions in the new repository's README.

<footer>

<!--
  <<< Author notes: Footer >>>
  Add a link to get support, GitHub status page, code of conduct, license link.
-->

---

Get help: [Post in our discussion board](https://github.com/orgs/skills/discussions/categories/code-with-copilot) &bull; [Review the GitHub status page](https://www.githubstatus.com/)

&copy; 2023 GitHub &bull; [Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/code_of_conduct.md) &bull; [MIT License](https://gh.io/mit)

</footer>
