<script>
	import Submit from 'svelte-material-icons/PlusCircle.svelte';
	import Copy from 'svelte-material-icons/Clipboard.svelte';
	import Facts from 'svelte-material-icons/Creation.svelte';
	import Security from 'svelte-material-icons/Security.svelte';

	import { slide, scale, fade } from 'svelte/transition';
	import { clipboard } from '@skeletonlabs/skeleton';

	export let password = '';
    export let combinations = "";
	export let passwordLength = 18;

	let alphabetCharacters =
		`a b c d e f g h i j k l m n o p q r s t u v w x y z A B C D E F G H I J K L M O P Q R S T U V W X Y Z 0 1 2 3 4 5 6 7 8 9 0 - + _ @ ! # $ % & * ( ) ' " [ ] ? ~  ; : ^ =`.split(
			' '
		);

	function generate_password() {
		let finalInput = '';
		password = '';

		while (
			password.match(
				`^(?=.*[A-Z].*[A-Z])(?=.*[!@#$&*])(?=.*[0-9].*[0-9])(?=.*[a-z].*[a-z].*[a-z]).{${passwordLength},}$`
			) == null
		) {
			finalInput = '';
			for (let index = 0; index < passwordLength; index++) {
				finalInput = `${finalInput}${
					alphabetCharacters[Math.floor(Math.random() * alphabetCharacters.length)]
				}`;
			}

			password = finalInput;
            combinations = get_password_length(password);
		}
	}

    /**
	    * @param {string} input
	*/
    function get_password_length(input) {
        let total_pool = 0;

        let hasLowerCase = false;
        let hasUpperCase = false;
        let hasSymbol = false;
        let hasNumber = false;

        const lowercase = "abcdefghijklmnopqrstuvwxyz".split("")
        const uppercase = "ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("")
        const numbers ="1234567890".split("")
        const symbols = `-+_@!#$%&*()'"[]?~;:^=`.split("")

        lowercase.forEach((entry) => {
            hasLowerCase = (input.includes(entry) ? true : hasLowerCase)
        })

        uppercase.forEach((entry) => {
            hasUpperCase = (input.includes(entry) ? true : hasUpperCase)
        })

        numbers.forEach((entry) => {
            hasNumber = (input.includes(entry) ? true : hasNumber)
        })

        symbols.forEach((entry) => {
            hasSymbol = (input.includes(entry) ? true : hasSymbol)
        })

        total_pool = (hasSymbol ? total_pool + 33 : total_pool);
        total_pool = (hasUpperCase ? total_pool + 26 : total_pool);
        total_pool = (hasLowerCase ? total_pool + 26 : total_pool);
        total_pool = (hasNumber ? total_pool + 10 : total_pool);

        return (total_pool ** input.split("").length).toExponential(2)
    }
</script>

<div class="w-5/6 mt-5 m-auto">
	<div class="text-center mt-10 select-none">
		<button type="button" on:click={generate_password} class="btn variant-filled">
			<span class="font-bold">Create</span>
			<Submit />
		</button>

		<div>
			{#if password}
				<div class="password-wrapper mt-5">
					<button use:clipboard={password}  transition:scale|local type="button" class="btn variant-ringed-primary h-12">
						<span class="font-bold">{password}</span>
						<Copy />
					</button>
				</div>
			{:else}
				<p  transition:slide|local class="mt-3 font-bold">No password has been generated.</p>
			{/if}
		</div>

		{#if password}
            <div transition:scale|local>
			    <Facts class="m-auto mt-5" size="50" />
            </div>

			<h4 class="font-bold italic">
				Facts about your <span class="font-bold dark:text-warning-600 text-yellow-400">new</span> password:
			</h4>

			<dl class="list mt-2 space-y-3" transition:slide|local>
				<div>
					<span class="text-gray-gray dark:text-gray-500"
						>Your password has <span
							class="font-bold dark:text-success-500 text-green-400">{combinations}</span
						> combinations.</span
					>
				</div>

				<div>
					<span class="text-gray-gray dark:text-gray-500"
						>Your password has upper -and lowercase characters. Making it harder to crack.</span
					>
				</div>

				<div>
					<span class="text-gray-700 dark:text-gray-500"
						>Your password is more unique and will not show up in common passwords.</span
					>
				</div>
			</dl>
		{:else}
            <div transition:slide|local>
                <div transition:scale|local>
                    <Security class="m-auto mt-10" size="50" />
                </div>

                <p class="w-1/3 m-auto mt-5 text-gray-700 dark:text-gray-500 italic"  transition:fade|local>
                    "Strong passwords are of the utmost importance. They protect your electronic accounts and
                    devices from unauthorized access, keeping your sensitive personal information safe. The more
                    complex the password, the more protected your information will be from cyber threats and
                    hackers."
                </p>
            </div>
		{/if}
	</div>
</div>
