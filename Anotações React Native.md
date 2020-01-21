**Dicas para desenvolvimento React Native**
___

Tutorial de MarkDown: [Blog do Da2k](https://blog.da2k.com.br/2015/02/08/aprenda-markdown/)
___
**1. Uso da função require** 

A função require( ) recebe apenas **strings estáticas**, ou seja, nada de tentar interpolar algo do tipo 

					require(`./assets/${imagePath}`) 
ou 

					require('./assets/'+imagePath)
Use algo desse tipo

						require('./assets/photo');

**2. Problemas no FlatList** 

Algumas vezes, ao usar FlatList, o último elemento tem problemas de visibilidade, podendo ficar "cortado". Para lidar com isso, adicionar um estilo à propriedade contentContainerStyle={{ paddingBottom: 100}}. Com um paddingBottom adequado, o último elemento poderá ser visualizado corretamente.

**3. Renderização de elementos** 

Não esqueça que corpo de função não renderiza Views se as mesmas não estiverem no retorno. Renderização deve ficar entre parênteses ( ) e não entre chaves { }.

**4. Geração de APK usando Expo**

							expo build:android
Aceite as recomendações do Expo
É bom estar com servidor aberto (expo start)
     
É possível obter SHA-1 e SHA-256 usando 

						expo fetch:android:hashes"

    
**5. Login Via Google**

   5.1. Instalar a dependência

        expo install expo-google-app-auth

5.2. Importar a dependência

        import * as Google from 'expo-google-app-auth';

5.3. Configurações

Criar projeto no Google Developer Console
Criar uma ID OAuth para aplicação Android, lembrando de no package, inserir **'host.exp.exponent'**
Ao fim do cadastro, será retornado um androidClientId

5.4. Como usar ?

Tenha no código uma variável de configuração com o androidClientId

        const config = {
            androidClientId: '149127590121-22lgkuovb1an7qjvcij1vtkeec3aj4c9.apps.googleusercontent.com',
            scopes: ['profile', 'email'],
        }

5.4.1. Login

            async function signInWithGoogleAsync() {
                try {
                  const result = await Google.logInAsync(config);
              
                  if (result.type === 'success') {

                    //result.user.name terá o nome do usuário
                    //result.user.email terá o e-mail do usuário
                    //result.user.photoUrl terá a url da foto de perfil do usuário
                    //result.accessToken terá o token de acesso usado em requisições HTTP
                    //result.type terá o status da operação de Login

                    return result.accessToken;
                  } else {
                    return { cancelled: true };
                  }
                } catch (e) {
                  return { error: true };
                }
            }

5.4.2. Logout

            async function logOutAsync (){
                if(type === 'success'){
                  await Google.logOutAsync({ accessToken, ...config });

                  //type = success sinaliza que havia um usuário logado
                  //Obviamente, só se pode deslogar um usuário logado
                }
                else{
                  alert('Sem usuário logado');
                }
            }

**6. Obter Localização**

6.1. Instalar a dependência:

        expo install expo-location

6.2. Importar no projeto:

        import { requestPermissionsAsync, getCurrentPositionAsync } from 'expo-location';

6.3. Como usar ?
    
        const { granted } = await requestPermissionsAsync();
        //Isso retorna um boolean (granted) que diz se o usuário deu ou não permissão

        //Caso o usuário tenha dado permissão, podemos obter as coordendas atuais do usuário
        const { coords } = await getCurrentPositionAsync({
            enableHighAccuracy: true,
        });
        const { latitude, longitude } = coords;

