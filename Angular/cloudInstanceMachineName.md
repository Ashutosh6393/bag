//----------------------Cloud Instance Machine Name----------------------

html

<h1>Create Instance</h1>
<section role="main">
  <article>
    <form class="form"
          [formGroup]="instanceCreationForm">
      <div class="form-group">
        <label for="name">Name:</label>
        <input type="text"
               id="name"
               formControlName="name"
               required/>
      </div>
      <div class="form-group">
        <label for="machine-name">Machine Name:</label>
        <input type="text"
               id="machine-name"
               disabled
               [value]="machineNameField"/>
        <p>This field is generated automatically</p>
      </div>
      <div class="form-actions">
        <button type="submit"
                [disabled]="instanceCreationForm.invalid">Next: Location
        </button>
      </div>
    </form>
  </article>
</section>


ts


import { Component } from '@angular/core';
import {
  FormControl,
  FormGroup
} from '@angular/forms';
import { InstanceService } from './services/instance.service';

export interface InstanceServiceToBeImplemented {
  translateNameToMachineName(name: string): string;
}

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: [
    './app.component.scss'
  ],
  standalone: false
})

export class AppComponent {
  //instanceService!: InstanceServiceToBeImplemented;
  constructor(public instanceService: InstanceService) {}


  instanceCreationForm: FormGroup = new FormGroup({
    name: new FormControl<string>('')
  });

  get machineNameField(): string {
    //return this.instanceCreationForm.controls.name.value;
      const name = this.instanceCreationForm.controls.name.value ;
      return this.instanceService.translateNameToMachineName(name);
  }
}


//Service.ts

export type Instance = {
  id: string, hostname: string
};

export const instanceList: Instance[] = [
  {
    'id': '8d75d0fe-33a8-49f8-a658-075191752b43',
    'hostname': '1und1.de'
  },
  {
    'id': '364d3764-8633-45f9-921a-d6d6c85272d4',
    'hostname': 'ning.com'
  },
  {
    'id': '5d0b664d-5ec4-4607-9743-b7a33a034123',
    'hostname': 'qq.com'
  },
  {
    'id': 'f2af7f56-0dc6-47a0-8076-e7c2974b6cd5',
    'hostname': 'eepurl.com'
  },
  {
    'id': '01f94e11-45f2-4c48-aa25-0adc47999713',
    'hostname': 'nhs.uk'
  },
  {
    'id': 'e8959340-f8e6-4a6a-b9be-e0ee7bf02e34',
    'hostname': 'arizona.edu'
  },
  {
    'id': 'f642e120-2391-4efa-be02-dd886d384b57',
    'hostname': 'chicagotribune.com'
  },
  {
    'id': '2571c786-2aac-4234-b6bd-95acd16c4e15',
    'hostname': 'oaic.gov.au'
  },
  {
    'id': '0d2db19c-b06c-4000-928b-267d163c7b05',
    'hostname': 'nationalgeographic.com'
  },
  {
    'id': '3f892464-d1a1-4c28-9b9a-728e09b45f2d',
    'hostname': 'irs.gov'
  },
  {
    'id': '03b57578-d487-448b-947f-7471f747ceb9',
    'hostname': 'goo.gl'
  },
  {
    'id': '7caf9b8b-52c7-417c-8efb-8bcc52bddbc3',
    'hostname': '163.com'
  },
  {
    'id': '709b869d-2770-4542-83b9-1fe344cc275a',
    'hostname': 'timesonline.co.uk'
  },
  {
    'id': '6a2fc89e-3c28-410b-b123-32c8c9ce1c9f',
    'hostname': 'ted.com'
  },
  {
    'id': '683f6a26-f044-4699-bf52-cbbe98e18bf6',
    'hostname': 'de.vu'
  },
  {
    'id': 'ee5e0929-945d-419a-b038-2339ce34b791',
    'hostname': 'mail.ru'
  },
  {
    'id': 'aa067792-3253-4e7c-8e99-afd60ad29d8a',
    'hostname': 'ebay.co.uk'
  },
  {
    'id': 'be44945a-1123-43ac-98c3-7f6f1c617cde',
    'hostname': 'issuu.com'
  },
  {
    'id': '8e6fd4d8-982b-4725-8840-a7f62c16ac8c',
    'hostname': 'photobucket.com'
  },
  {
    'id': '56c76546-79d6-4e99-9845-eec1ae0f0e81',
    'hostname': 'tmall.com'
  },
  {
    'id': '264472fd-3d66-40df-b591-5016c73ffdac',
    'hostname': 'alibaba.com'
  },
  {
    'id': 'bf573a2c-f8fa-4422-bbc5-d758d9a2c804',
    'hostname': 'twitpic.com'
  },
  {
    'id': '28e170bf-02b8-4fe5-b144-8ace103d16c5',
    'hostname': 'wufoo.com'
  },
  {
    'id': '8c3fe57d-0b18-4bf0-aa60-894f3ce1f9be',
    'hostname': 'smh.com.au'
  },
  {
    'id': 'e0c50ee9-ef37-4a74-90f1-160f7c2106c1',
    'hostname': 'arstechnica.com'
  },
  {
    'id': '36cb1d54-72e6-436a-8efd-ecf7bac5023d',
    'hostname': 'shareasale.com'
  },
  {
    'id': '4b3eb2a6-8d78-4b91-a878-5589929749d0',
    'hostname': 'va.gov'
  },
  {
    'id': '13faf1da-a8e0-44f4-9dbd-1ac4bd3817d6',
    'hostname': 'nyu.edu'
  },
  {
    'id': 'a022623d-4f28-4e29-8ef3-c4673239a8cb',
    'hostname': 'bluehost.com'
  },
  {
    'id': '1412cd78-c1f1-4e07-ac2f-478085723e9f',
    'hostname': 'squarespace.com'
  },
  {
    'id': '7eae218e-ffcd-4416-bcff-14bca8280667',
    'hostname': 'buzzfeed.com'
  },
  {
    'id': '1d16c5e2-471f-4c70-8354-58c9e7c14df4',
    'hostname': 'si.edu'
  },
  {
    'id': 'a284ae55-58ee-4d1a-9a17-3ac3a0b58247',
    'hostname': 'weebly.com'
  },
  {
    'id': 'eff2985a-a519-46df-aeef-47b1e4571e03',
    'hostname': 'csmonitor.com'
  },
  {
    'id': 'de77206b-d9c5-4249-8b1a-02db68ac9ba0',
    'hostname': 'miitbeian.gov.cn'
  },
  {
    'id': 'f6e60cc3-312e-407d-971b-c0bd7f07d67c',
    'hostname': 'list.manage.com'
  },
  {
    'id': '311cce5c-b0b2-4bae-af97-240df7fb8c38',
    'hostname': 'dyndns.org'
  },
  {
    'id': '93265877-f1cb-45e7-b90f-53a389076775',
    'hostname': 'businessweek.com'
  },
  {
    'id': '0d274077-d55c-42fb-ad74-fe6ed1d8ce83',
    'hostname': 'parallels.com'
  },
  {
    'id': '25dc3c68-b19e-46e7-8caa-2336177a012d',
    'hostname': 'patch.com'
  },
  {
    'id': '47d11c61-a6d9-4b0b-b5fb-148132e4841c',
    'hostname': 'biblegateway.com'
  },
  {
    'id': '0f4c7ae1-27e8-4c36-80cb-f93a0a80fe2e',
    'hostname': 'patch.com'
  },
  {
    'id': 'f43f8637-d095-4365-a67b-9181d8ce6bfe',
    'hostname': 'archive.org'
  },
  {
    'id': 'db845003-5d1d-4c88-893e-86bd57170f96',
    'hostname': 'smh.com.au'
  },
  {
    'id': 'cd222d31-4604-4f8a-a9e3-649eb042269d',
    'hostname': 'npr.org'
  },
  {
    'id': '2d6fc757-6bb8-4276-9b8f-ad84012cb1c7',
    'hostname': 'toplist.cz'
  },
  {
    'id': 'd118f273-cad8-4a1f-9297-df8dc1b4fa56',
    'hostname': 'twitpic.com'
  },
  {
    'id': '184dda8d-eccc-47a3-8922-5cc2b79b07d4',
    'hostname': 'skype.com'
  },
  {
    'id': 'fc18fcc1-a851-43a2-83c0-c7eb6eb007d8',
    'hostname': 'telegraph.co.uk'
  },
  {
    'id': 'd184dcfb-0183-44f3-8cd2-9a4caf577440',
    'hostname': 'mysql.com'
  },
  {
    'id': '1c58506f-620e-4946-87c8-ba2ac5b809bd',
    'hostname': 'bandcamp.com'
  },
  {
    'id': '83b0c238-0967-46ff-b1e8-a156f9f7f649',
    'hostname': 'qq.com'
  },
  {
    'id': 'ee7e1fa8-6314-4d57-853f-3c9650b3d24f',
    'hostname': 'bbc.co.uk'
  },
  {
    'id': '9e996153-59b5-4b42-90b8-ffac210a8699',
    'hostname': 'storify.com'
  },
  {
    'id': 'dfd589f3-33b9-49c0-9bf0-d6a0da7b6580',
    'hostname': 'mac.com'
  },
  {
    'id': 'e8a69172-6403-4305-9ee0-5ff5c818a275',
    'hostname': 'umich.edu'
  },
  {
    'id': 'e3876265-7e51-4c2e-9129-d04678695b71',
    'hostname': 'seesaa.net'
  },
  {
    'id': '7dc76e1c-91c5-4934-8d72-d65d65bf3a34',
    'hostname': 'apple.com'
  },
  {
    'id': 'b59a4fe7-f3e8-4c40-9da1-fb634b176976',
    'hostname': 'mail.ru'
  },
  {
    'id': 'b8681148-85fd-448d-bad5-2a2a0f0a13f0',
    'hostname': 'mac.com'
  },
  {
    'id': '2624fac9-bb39-42a1-b2a9-8544ab1216ac',
    'hostname': 'blog.com'
  },
  {
    'id': 'd3897404-7d81-4de5-935d-1b581b01f4b4',
    'hostname': 'yelp.com'
  },
  {
    'id': '5af21309-dbe8-44cd-84ac-8b020169b6fd',
    'hostname': 'zimbio.com'
  },
  {
    'id': 'cf59884f-9ef9-419d-81d1-d4d82028fcbd',
    'hostname': 'patch.com'
  },
  {
    'id': '9335b0ee-8ef0-4558-aacb-800358b9e08e',
    'hostname': 'long.domain.live.com'
  },
  {
    'id': 'c99faec8-3c35-4514-9c66-33995eee9e32',
    'hostname': 'newyorker.com'
  },
  {
    'id': 'fa845dac-33e4-45b5-b2e9-c9b288210dba',
    'hostname': 'quantcast.com'
  },
  {
    'id': '8780f422-f5a5-4f44-826b-51b739fdcb8d',
    'hostname': 'jiathis.com'
  },
  {
    'id': 'abb657ec-ae2b-45ca-8ad6-d969d5bb21a3',
    'hostname': '360.cn'
  },
  {
    'id': 'e7fa901c-f4d9-473b-a811-ce3b69f8795b',
    'hostname': 'cam.ac.uk'
  },
  {
    'id': '6641f8e9-48f3-40c6-8c37-62f20cbcc0d9',
    'hostname': 'fda.gov'
  },
  {
    'id': '84e605d6-b0b2-41bf-b0b4-5ef7a41800fe',
    'hostname': 'yolasite.com'
  },
  {
    'id': '284fb180-1b90-448b-9182-49f3c0f68cab',
    'hostname': 'adobe.com'
  },
  {
    'id': '73343e26-cab4-4f71-9347-0e0a3d49f4f7',
    'hostname': 'ustream.tv'
  },
  {
    'id': '5734f8c8-25f6-4e0e-8c8e-5bbc1cdcdc7a',
    'hostname': 'ox.ac.uk'
  },
  {
    'id': 'd996ae7e-6e7a-4894-b5e3-76b3956e9aea',
    'hostname': 'uol.com.br'
  },
  {
    'id': '2fc14707-bc07-45ec-9a67-034fa4115e98',
    'hostname': 'hubpages.com'
  },
  {
    'id': 'f3760e55-dd53-4b7f-aa3a-fa46d1171d44',
    'hostname': '360.cn'
  },
  {
    'id': '2ab081e3-27dd-422a-be54-f0b24e913226',
    'hostname': 'techcrunch.com'
  },
  {
    'id': 'e1638fcc-fe6e-4271-8c8e-f1c1c187559b',
    'hostname': 'smh.com.au'
  },
  {
    'id': '1e1b6842-6df1-4369-a1ab-2771cfd27f06',
    'hostname': 'imageshack.us'
  },
  {
    'id': '7abaef4c-841d-4481-bbd5-8f1c8c36d80b',
    'hostname': 'ask.com'
  },
  {
    'id': '9141f03c-1265-4b85-8d90-70c8ecd5d8b9',
    'hostname': '163.com'
  },
  {
    'id': 'f9b94f78-b8c7-4120-afe8-b57b2dfb9a06',
    'hostname': 'xing.com'
  },
  {
    'id': 'c00177e5-7b32-41ab-b7fa-afc4182713ed',
    'hostname': 'infoseek.co.jp'
  },
  {
    'id': 'f339e022-07e8-40e2-b63a-03568a409614',
    'hostname': 'wikimedia.org'
  },
  {
    'id': '520200c0-df07-4177-8850-c4dd07ca99b4',
    'hostname': 'bloomberg.com'
  },
  {
    'id': 'ee5766e8-c6d6-480e-b9ef-c8cde5cc9309',
    'hostname': 'dedecms.com'
  },
  {
    'id': 'd9f7e0de-8253-465f-ae4d-6f29f8145d77',
    'hostname': 'prlog.org'
  },
  {
    'id': '321a9ae2-0f7f-452d-8487-c55557baab19',
    'hostname': 'yale.edu'
  },
  {
    'id': '686f3073-2c94-4364-8e2b-7288d4c530d4',
    'hostname': 'bloglovin.com'
  },
  {
    'id': '3e8814b2-c392-4d63-86ca-586982d8bfff',
    'hostname': 'infoseek.co.jp'
  },
  {
    'id': '4ee24f36-6070-44ae-a0a2-59e75fc3298d',
    'hostname': 'ning.com'
  },
  {
    'id': '3ad25f4d-8598-44a6-bd83-9795a45b5bf6',
    'hostname': 'drupal.org'
  },
  {
    'id': '58939377-41bb-4f19-9750-9ec26c6b5141',
    'hostname': 'ucoz.com'
  },
  {
    'id': '4207544d-6748-4cd3-92f0-7615f9949165',
    'hostname': 'google.com.hk'
  },
  {
    'id': '4446da62-dc53-47e9-a02c-df551dac7053',
    'hostname': 'soundcloud.com'
  },
  {
    'id': '4c961cca-d447-425b-838a-e35766f3247a',
    'hostname': 'mlb.com'
  },
  {
    'id': 'a1ca703f-0f0b-412c-a043-c929944de4b9',
    'hostname': 'tiny.cc'
  },
  {
    'id': '56224092-a6d1-43c4-8128-1e04ddefa74f',
    'hostname': 'sogou.com'
  }
];
