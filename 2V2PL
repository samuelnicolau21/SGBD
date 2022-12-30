#include <stdio.h>
#include <stdlib.h>
#include <string.h>


typedef struct operacao {char tipo_operacao;                         //struct de uma operacao
                         char id_transacao;
                         char objeto[2];
                         int escalonado;
                         struct operacao *operacao_seguinte;}operacao;

typedef struct linha_syslock {                                       //struct de uma linha
                            char l_id_transacao;
                            char l_obejto[2];
                            char l_bloqueio[2];
                            int l_granulosidade;
                            char l_status;
                            char l_bloqueador;
                            struct linha_syslock * linha_seguinte;}linha;

void main(){

//VARIAVEIS
operacao *primeiro,*x, *first, *s, *comeco, *a;
linha *inicio, *y, *aux;
char op,id_t,obj[2],i,c[4];
int soma=0;


inicio=(linha*)malloc(sizeof(linha));
inicio->l_id_transacao='0';

first=(operacao*)malloc(sizeof(operacao));
first->id_transacao='0';


comeco=(operacao*)malloc(sizeof(operacao));
comeco->id_transacao='0';


// 1- LÊ A PRIMEIRA OPERAÇÃO

primeiro=malloc(sizeof(operacao));
scanf("%c",&op);
primeiro->tipo_operacao=op;
scanf("%c",&id_t);
primeiro->id_transacao=id_t;
if(primeiro->tipo_operacao!='c' ||primeiro->tipo_operacao!='C'){scanf("%s",obj);
strcpy(primeiro->objeto,obj);}
scanf("%c",&i);
primeiro->escalonado=1;
primeiro->operacao_seguinte=malloc(sizeof(operacao));
x=primeiro->operacao_seguinte;


// 2- LÊ AS PRÓXIMAS OPERAÇÕES
while(i!='\n'){


                scanf("%c",&i);
                x->tipo_operacao=i;
                scanf("%c",&id_t);
                x->id_transacao=id_t;
                if(x->tipo_operacao!='c' ||x->tipo_operacao!='C'){
                scanf("%s",obj); strcpy(x->objeto,obj);}
                scanf("%c",&i);
                x->escalonado=1;
                 x->operacao_seguinte=malloc(sizeof(operacao));
                 x=x->operacao_seguinte;
                                        }

x->id_transacao = '0';

// 3- PRINTA TODAS AS OPERAÇÕES
x=primeiro;
while(x->id_transacao!= '0'){
    printf("%c%c%s ",x->tipo_operacao,x->id_transacao,x->objeto);     // percorre e mostra a lista encadeada até o último elemento
    x=x->operacao_seguinte;

}

soma=0;
x=primeiro;
while(x->id_transacao!='0'){
    soma=soma+x->escalonado;
    x=x->operacao_seguinte;
}
printf("\n a soma das operações não escalonadas eh %d", soma);
//4- ANALISA AS OPERAÇÕES
while(soma!=0){//deve se repetir enquanto a soma dos campos escalonado na lista operações for diferente de zero

x=primeiro;
while(x->id_transacao!= '0'){
        //SE A OPERAÇÃO É DE ESCRITA
    if((x->tipo_operacao=='W'||x->tipo_operacao=='w'||x->tipo_operacao=='E'||x->tipo_operacao=='e')&& x->escalonado==1){
        i='0';
        printf("\n eh um comando de escrita");
        //LÊ A SYSLOCK E VERIFICA SE EXISTE ALGUM BLOQUEIO INCOMPATÍVEL
        y=inicio;
        while(y->l_id_transacao!='0'){
            //VERIFICA SE HÁ BLOQUEIO INCOMPATÍVEL
                printf("\nESTOU VERIFICANDO SE EXISTE BLOQUEIO INCOMPATÍVEL");
            if( (strcmp(y->l_bloqueio,"w")==0 || strcmp(y->l_bloqueio,"c")==0) && strcmp(y->l_obejto,x->objeto)==0 && y->l_id_transacao!=x->id_transacao && y->l_status=='1' && x->escalonado==1){
                i='1';
                printf("\nEXISTE BLOQUEIO INCOMPATÍVE");
            }

            y=y->linha_seguinte;
        }
        //SE TEM BLOQUEIO INCOMPATÍVEL
        if(i=='1'){

            //LÊ ATÉ A ÚLTIMA LINHA DA SYSLOCK
            y=inicio;
            while(y->l_id_transacao!='0'){

            //ANOTANDO QUAL A TRANSAÇÃO QUE ESTÁ RETENDO O BLOQUEIO
            if((strcmp(y->l_bloqueio,"w")==0 || strcmp(y->l_bloqueio,"c")==0 ) && y->l_status=='1' && strcmp(y->l_obejto,x->objeto )==0 && y->l_id_transacao!=x->id_transacao){
                c[0]=y->l_id_transacao;}
            y=y->linha_seguinte;}


            //E CRIA UMA NOVA LINHA COM STATUS 3 LÁ
            y->l_id_transacao=x->id_transacao;
            strcpy(y->l_obejto,x->objeto);
            strcpy(y->l_bloqueio,"w");//bloqueio de leitura
            y->l_granulosidade='1';//a granulosidade deve ser dado a partir de um teste
            y->l_status='3';
            y->l_bloqueador=c[0];
            y->linha_seguinte=(linha*)malloc(sizeof(linha));
            y=y->linha_seguinte;
            y->l_id_transacao='0';


            //E CRIA UMA LINHA
            //a=comeco;
            //while(a->id_transacao!='0'){ a=a->operacao_seguinte; }

            //NO FIM DA  LISTA DE ESPERA
            //a->id_transacao=x->id_transacao;
           // strcpy(a->objeto,x->objeto);
            //a->tipo_operacao='w';
            //a->operacao_seguinte=(operacao*)malloc(sizeof(operacao));
            //a=a->operacao_seguinte;
            //a->id_transacao='0';


            printf("\nadcionei uma linha no fim da syslock e no fim da lista de espera porque tem bloqueio incompatível");
                i='0';
            }

        //SE NÃO TEM BLOQUEIO INCOMPATÍVEL
        else{
            //PERCORRE ATÉ O FIM DA SYSLOCK
            y=inicio;
            while(y->l_id_transacao!='0'){y=y->linha_seguinte;}
            //E CRIA UMA NOVA LINHA COM STATUS 1 LÁ
            y->l_id_transacao=x->id_transacao;
            strcpy(y->l_obejto,x->objeto);
            strcpy((linha*)y->l_bloqueio,"w");//bloqueio de leitura
            y->l_granulosidade='1';//a granulosidade deve ser dado a partir de um teste
            y->l_status='1';
            y->l_bloqueador='0';
            y->linha_seguinte=(linha*)malloc(sizeof(linha));
            y=y->linha_seguinte;
            y->l_id_transacao='0';
            //PERCORRE ATÉ O FIM DO ESCALONAMENTO
            s=first;
            while(s->id_transacao!='0'){ s=s->operacao_seguinte; }

            //E CRIA UMA LINHA LÁ
            s->id_transacao=x->id_transacao;
            strcpy(s->objeto,x->objeto);
            s->tipo_operacao='w';
            s->operacao_seguinte=(operacao*)malloc(sizeof(operacao));
            s=s->operacao_seguinte;
            s->id_transacao='0';
            x->escalonado=0;
            printf("\nadcionei uma linha na syslock e uma linha no escalonamento final  porque NNNNN tinha bloqueio incompatível");

            }

    }

        //SE A OPERAÇÃO É DE LEITURA
    if((x->tipo_operacao=='R'||x->tipo_operacao=='r'||x->tipo_operacao=='L'||x->tipo_operacao=='l')&& x->escalonado==1){
        i='0';
        printf("\n eh um comando de leitura");

        //LÊ A SYSLOCK E VERIFICA SE EXISTE ALGUM BLOQUEIO INCOMPATÍVEL
        y=inicio;
        while(y->l_id_transacao!='0'){

            //VERIFICA SE HÁ BLOQUEIO INCOMPATÍVEL
                printf("\nESTOU VERIFICANDO SE EXISTE BLOQUEIO INCOMPATÍVEL");
            if( strcmp(y->l_bloqueio,"c")==0 && strcmp(y->l_obejto,x->objeto)==0 && y->l_id_transacao != x->id_transacao){
                i='1';
                printf("\nEXISTE BLOQUEIO INCOMPATÍVE");
            }

            y=y->linha_seguinte;
        }
        //SE TEM BLOQUEIO INCOMPATÍVEL
        if(i=='1'){

            //LÊ ATÉ A ÚLTIMA LINHA DA SYSLOCK
            y=inicio;
            while(y->l_id_transacao!='0'){

            //ANOTANDO QUAL A TRANSAÇÃO QUE ESTÁ RETENDO O BLOQUEIO
            if(strcmp(y->l_bloqueio,"c")==0  && y->l_status=='1' && strcmp(y->l_obejto,x->objeto )==0 && y->l_id_transacao!=x->id_transacao){
                c[0]=y->l_id_transacao;}
            y=y->linha_seguinte;}


            //E CRIA UMA NOVA LINHA COM STATUS 3 LÁ
            y->l_id_transacao=x->id_transacao;
            strcpy(y->l_obejto,x->objeto);
            strcpy(y->l_bloqueio,"r");//bloqueio de leitura
            y->l_granulosidade='1';//a granulosidade deve ser dado a partir de um teste
            y->l_status='3';
            y->l_bloqueador=c;
            y->linha_seguinte=(linha*)malloc(sizeof(linha));
            y=y->linha_seguinte;
            y->l_id_transacao='0';


            //E CRIA UMA LINHA
            //a=comeco;
            //while(a->id_transacao!='0'){ a=a->operacao_seguinte; }

            //NO FIM DA  LISTA DE EESPERA
            //a->id_transacao=x->id_transacao;
            //strcpy(a->objeto,x->objeto);
            //a->tipo_operacao='r';
           // a->operacao_seguinte=(operacao*)malloc(sizeof(operacao));
            //a=a->operacao_seguinte;
           // a->id_transacao='0';


            printf("\nadcionei uma linha no fim da syslock e no fim da lista de espera porque tem bloqueio incompatível");
                i='0';
            }

        //SE NÃO TEM BLOQUEIO INCOMPATÍVEL
        else{
            //PERCORRE ATÉ O FIM DA SYSLOCK
            y=inicio;
            while(y->l_id_transacao!='0'){y=y->linha_seguinte;}
            //E CRIA UMA NOVA LINHA COM STATUS 1 LÁ
            y->l_id_transacao=x->id_transacao;
            strcpy(y->l_obejto,x->objeto);
            strcpy((linha*)y->l_bloqueio,"r");//bloqueio de leitura
            y->l_granulosidade='1';//a granulosidade deve ser dado a partir de um teste
            y->l_status='1';
            y->l_bloqueador='0';
            y->linha_seguinte=(linha*)malloc(sizeof(linha));
            y=y->linha_seguinte;
            y->l_id_transacao='0';


            //PERCORRE ATÉ O FIM DO ESCALONAMENTO
            s=first;
            while(s->id_transacao!='0'){ s=s->operacao_seguinte; }

            //E CRIA UMA LINHA LÁ
            s->id_transacao=x->id_transacao;
            strcpy(s->objeto,x->objeto);
            s->tipo_operacao='r';
            s->operacao_seguinte=(operacao*)malloc(sizeof(operacao));
            s=s->operacao_seguinte;
            s->id_transacao='0';
            x->escalonado=0;
            printf("\nadcionei uma linha na syslock e uma linha no escalonamento final  porque NNNNN tinha bloqueio incompatível");

            }

    }

        //SE A OPERAÇÃO É DE COMMIT
    if((x->tipo_operacao=='C'||x->tipo_operacao=='c')&& x->escalonado==1){
        i='0';
        printf("\n eh um comando de commit");
        //VERIFICA SE EXISTE W COM STATUS 1 PARA A TRANSAÇÃO
        printf("\n estou verificando se existe w para converter");

        y=inicio;
        while(y->l_id_transacao!='0'){
            if(strcmp(y->l_bloqueio,"w")==0 && y->l_status=='1' && strcmp(y->l_id_transacao,x->id_transacao)==0){
                   printf("\n existe w para converter");
                    c[1]=x->objeto;
                    i='1';
            }
            y=y->linha_seguinte;
        }

        //SE SIM-> VERIFICA SE EXISTE INCOMPATIBILIDADE
        if(i=='1'){
            printf("\n estou verificando se tem incompatibilidade para fazer a conversao");
            aux=inicio;
            c[3]='0';
            while(aux->l_id_transacao!='0'){
            //SE TEM BLOQUEIO INCOMPATÍVEL
            if(aux->l_id_transacao!=x->id_transacao && strcmp(aux->l_obejto,c[1])==0 && strcmp(aux->l_bloqueio,"r")==0 && aux->l_status=='1'){
                        printf("\n tinha incompatibilidade para a conversao");
                        c[2]=aux->l_bloqueador;//GUARDA O ID DO BLOQUEADOR
                        c[3]='1';//REGISTRA QUE HÁ BLOQUEIOS INCOMPATÍVEIS
                        }

                    aux=aux->linha_seguinte;}

            if(c[3]=='1'){
           //PERCORRE A SYSLOCK E LÁ
           aux=inicio;
           while(aux->l_id_transacao!='0'){
           aux=aux->linha_seguinte;}
           //NO FIM ADCIONA A LINHA DO COMMIT COM STATUS 2
            aux->l_id_transacao=x->id_transacao;
            strcpy(aux->l_obejto,x->objeto);
            strcpy(aux->l_bloqueio,"c");//bloqueio de commit
            aux->l_granulosidade='1';//a granulosidade deve ser dado a partir de um teste
            aux->l_status='2';
            aux->l_bloqueador=c;
            aux->linha_seguinte=(linha*)malloc(sizeof(linha));
            aux=aux->linha_seguinte;
            aux->l_id_transacao='0';
            printf("\n adcionei uma linha de commit na syslock com status 2");}


        }
        //SE SIM E SE NÃO TEM SINAL VERMELHO
        if(i=='1' && c[3]!='1'){
           //PERCORRE A SYSLOCK E VAI CONVERTENDO TODOS OS W DA TRANSAÇÃO EM Cs E VAI LIBERANDO TODOS OS BLOQUEIOS
           aux=inicio;
           while(aux->l_id_transacao!='0'){
                if(aux->l_id_transacao==x->id_transacao && aux->l_bloqueio=='w' && aux->l_status=='1'){
                    strcpy(aux->l_bloqueio,"c");
                    aux->l_status='0';
                }
           aux=aux->linha_seguinte;}

        // E NO FIM DA SYSLOCK CRIA O REGISTRO
        aux->l_id_transacao=x->id_transacao;
        strcpy(aux->l_obejto,x->objeto);
        strcpy((linha*)aux->l_bloqueio,"c");//bloqueio de leitura
        aux->l_granulosidade='1';//a granulosidade deve ser dado a partir de um teste
        aux->l_status='0';
        aux->l_bloqueador='0';
        aux->linha_seguinte=(linha*)malloc(sizeof(linha));
        aux=aux->linha_seguinte;
        aux->l_id_transacao='0';
        printf("\n adcionei uma linha de commit na syslock com status 0");
        //DO COMMIT  DEIXA O STATUS DELE ZERADO E MANDA O COMMIT PARA O ESCALONAMENTO
        //PERCORRE ATÉ O FIM DO ESCALONAMENTO
            s=first;
            while(s->id_transacao!='0'){ s=s->operacao_seguinte; }

            //E CRIA UMA LINHA LÁ
            s->id_transacao=x->id_transacao;
            strcpy(s->objeto,x->objeto);
            s->tipo_operacao='c';
            s->operacao_seguinte=(operacao*)malloc(sizeof(operacao));
            s=s->operacao_seguinte;
            s->id_transacao='0';
            x->escalonado=0;
            printf("\n adcionei uma linha de commit no escalonamento");
        }
        //SE NÃO-> VARRE A SYSLOCK LIBERANDO TODOS OS BLOQUEIOS DE LEITURA, NO FIM DA SYSLOCK
        //CRIA O REGISTRO DO COMMIT  COM STATUS ZERADO E MANDA O COMMIT PARA O ESCALONAMENTO
        else if(i!=1){
            //PERCORRE A SYSLOCK E VAI CONVERTENDO TODOS OS W DA TRANSAÇÃO EM Cs E VAI LIBERANDO TODOS OS BLOQUEIOS
           aux=inicio;
           while(aux->l_id_transacao!='0'){
                if(aux->l_id_transacao==x->id_transacao && aux->l_bloqueio=='w' && aux->l_status=='1'){
                    strcpy(aux->l_bloqueio,"c");
                    aux->l_status='0';
                }
           aux=aux->linha_seguinte;}

        // E NO FIM DA SYSLOCK CRIA O REGISTRO
        aux->l_id_transacao=x->id_transacao;
        strcpy(aux->l_obejto,x->objeto);
        strcpy((linha*)aux->l_bloqueio,"c");//bloqueio de leitura
        aux->l_granulosidade='1';//a granulosidade deve ser dado a partir de um teste
        aux->l_status='0';
        aux->l_bloqueador='0';
        aux->linha_seguinte=(linha*)malloc(sizeof(linha));
        aux=aux->linha_seguinte;
        aux->l_id_transacao='0';
        printf("\n adcionei uma linha de commit na syslock com status 0");

        //DO COMMIT  DEIXA O STATUS DELE ZERADO E MANDA O COMMIT PARA O ESCALONAMENTO
        //PERCORRE ATÉ O FIM DO ESCALONAMENTO
            s=first;
            while(s->id_transacao!='0'){ s=s->operacao_seguinte; }

            //E CRIA UMA LINHA LÁ
            s->id_transacao=x->id_transacao;
            strcpy(s->objeto,x->objeto);
            s->tipo_operacao='c';
            s->operacao_seguinte=(operacao*)malloc(sizeof(operacao));
            s=s->operacao_seguinte;
            s->id_transacao='0';
            x->escalonado=0;
            printf("\n adcionei uma linha de commit no escalonamento");
        }

            }

x=x->operacao_seguinte;
}

soma=0;
x=primeiro;
while(x->id_transacao!='0'){
    soma=soma+x->escalonado;
    x=x->operacao_seguinte;
}
printf("\n a soma das operações não escalonadas eh %d", soma);

}
//PRINTANDO A SYSLOCK INFO
y=inicio;
printf("\nPRINTANDO A SYSLOCK INFO\n");
while(y->l_id_transacao!='0'){
        printf("\n idt:%c objeto:%s bloqueio:%s status:%c retedor do bloqueio:%c  ",y->l_id_transacao,y->l_obejto,y->l_bloqueio,y->l_status,y->l_bloqueador);
    y=y->linha_seguinte;
}

//PRINTANDO A LISTA DO ESCALONAMENTO FINAL
s=first;
printf("\nPRINTANDO A LISTA DO ESCALONAMENTO FINAL\n");
while(s->id_transacao!='0'){
    printf("%c%c%s ",s->tipo_operacao,s->id_transacao,s->objeto);
    s=s->operacao_seguinte; }



}
