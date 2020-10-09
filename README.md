<!DOCTYPE html>
<html>
<head>
	<title>My First OAuth2 App</title>
</head>
<body>
	<div id="info">
		Hoi!
	</div>
	<a id="login" style="display: none;" href="https://discord.com/oauth2/authorize?client_id=706944478681890817&permissions=1153&redirect_uri=http://localhost:53134&response_type=code&scope=guilds.join">Identify Yourself</a>
	<script>
		window.onload = () => {
			const fragment = new URLSearchParams(window.location.hash.slice(1));

			if (fragment.has("access_token")) {
				const accessToken = fragment.get("access_token");
				const tokenType = fragment.get("token_type");
                console.log(accessToken)

				fetch('https://discord.com/api/users/@me', {
					headers: {
						authorization: `${tokenType} ${accessToken}`
					}
				})
					.then(res => res.json())
					.then(response => {
						const { username, discriminator } = response;
						document.getElementById('info').innerText += ` ${username}#${discriminator}`;
					})
					.catch(console.error);

			}
			else {
				document.getElementById('login').style.display = 'block';
			}
		}
	</script>
</body>
</html>
